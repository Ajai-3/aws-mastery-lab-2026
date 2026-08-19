# DevOps Scenario Interview — Q&A with Follow-ups

[<- Back to Main 3-Tier Architecture Guide](./aws-3-tier-architecutre.md)

---

## Q1: EKS pods at 95% CPU, users still complaining about slow response. What do you check first?

**Answer**
- Check ALB health checks first — are pods actually responding, or timing out?
- Check HPA (Horizontal Pod Autoscaler) — is it scaling pods up, or stuck at max replicas?
- Check the database — high app CPU often means the app is *waiting* on slow DB queries, not doing real work
- Check Redis cache hit rate — if cache is failing, every request hits the DB directly, which slows everything down

**Simple summary:** High CPU doesn't always mean "add more servers." First find out WHY it's high — slow DB, cache miss, or bad code — then fix that, not just scale blindly.

**Follow-up questions to expect:**
- "You added more pods and it's still slow — what does that tell you?" (Answer: bottleneck isn't compute, it's the DB or cache)
- "How do you find out which specific query is slow?" (Answer: `EXPLAIN ANALYZE` in Aurora, check slow query logs)

---

## Q2: Your database is under heavy load but app servers have low CPU. What's happening?

**Answer**
- App servers are fine — this means the problem is DB-side, not app-side
- Common causes:
  - No caching — every request hits Aurora directly instead of Redis
  - Missing database indexes — queries scan entire tables instead of using fast lookups
  - No connection pooling — too many open connections exhausting the DB's limit
  - N+1 query problem — app code making many small queries instead of one efficient one

**Simple summary:** App is healthy, DB is drowning. Fix: add caching (Redis), add indexes, use connection pooling (PgBouncer/RDS Proxy).

**Follow-up questions to expect:**
- "How do you know if it's missing an index?" (Answer: run `EXPLAIN ANALYZE`, look for "sequential scan" instead of "index scan")
- "What if adding indexes doesn't help enough?" (Answer: add a read replica, or push more reads to Redis cache)

---

## Q3: Aurora primary DB goes down at 3am. What happens, and what do you check?

**Answer**
- If Multi-AZ is set up correctly: Aurora automatically fails over to the standby replica — usually under 60 seconds, minimal downtime
- What to check first:
  - Did the failover actually complete? (check AWS console/CloudWatch alarms)
  - Are app pods reconnecting to the new primary automatically, or are they stuck with a cached old connection?
  - Check application error logs — some apps need a restart to pick up the new DB endpoint

**Simple summary:** Multi-AZ means the backup takes over automatically. Your job is to confirm it actually worked and that the app reconnected — not to manually fix the database.

**Follow-up questions to expect:**
- "What if Multi-AZ wasn't configured?" (Answer: manual restore from backup — much longer downtime, this is why Multi-AZ matters)
- "How do you prevent this from happening again?" (Answer: can't prevent hardware failure, but Multi-AZ + automated backups + alerting minimizes impact)

---

## Q4: A developer pushed AWS credentials to a public GitHub repo. Walk me through your response.

**Answer**
- Step 1: Immediately rotate/revoke the exposed credentials in AWS IAM — assume they're already compromised the moment they're public
- Step 2: Check CloudTrail logs — did anyone actually use those credentials before you revoked them?
- Step 3: Remove the credentials from git history (not just delete the file — old commits still contain it)
- Step 4: Review what that credential had access to — did the damage reach further, like S3 buckets or the database?
- Step 5 (prevention): Move to Secrets Manager + IRSA so credentials never live in code again; add pre-commit hooks or GitHub secret scanning to catch this before it's pushed

**Simple summary:** Revoke first, investigate second, clean up third, prevent it happening again last.

**Follow-up questions to expect:**
- "Why revoke before investigating?" (Answer: every minute the credential stays active is more risk — speed matters more than certainty here)
- "How would IRSA have prevented this?" (Answer: IRSA gives pods temporary auto-rotating credentials — nothing hardcoded to leak in the first place)

---

## Q5: Prove to an auditor that your database cannot be reached from the public internet.

**Answer**
- Show the VPC setup: database sits in a **private subnet** with no route to an internet gateway
- Show the security group rules: only allows inbound traffic from the app tier's security group — not from `0.0.0.0/0` (which means "anywhere")
- Show there's no public IP assigned to the Aurora instance
- Optional: run a test — try to connect to the DB from outside the VPC and show it fails/times out

**Simple summary:** Private subnet + locked-down security group + no public IP = the database is physically unreachable from the internet, not just "protected by a password."

**Follow-up questions to expect:**
- "What's the difference between a security group and a NACL?" (Security group = stateful, per-instance. NACL = stateless, per-subnet, checks both directions)
- "What if someone needs emergency access to the DB?" (Answer: bastion host or AWS Systems Manager Session Manager — never open the DB directly)

---

## Q6: Why use IRSA instead of an IAM user with access keys for EKS pods?

**Answer**
- Access keys are long-lived — if leaked, they work until someone manually revokes them
- IRSA gives each pod short-lived, auto-rotating credentials tied to its Kubernetes service account
- No secret is stored anywhere — not in code, not in a config file, not in an environment variable
- If a pod is compromised, the blast radius is limited to only what that specific pod's role allows — not a shared key used everywhere

**Simple summary:** Access keys are a static password that can leak. IRSA is a temporary badge that expires and is scoped to exactly one pod's job.

**Follow-up questions to expect:**
- "What happens if IRSA credentials are somehow stolen?" (Answer: they expire quickly — much smaller window of risk than a static key)
- "Does every pod need its own IAM role?" (Answer: ideally yes — least privilege, one pod compromised shouldn't unlock the whole system)

---

## Q7: Walk me through what happens end-to-end when a user clicks "buy now."

**Answer — trace it service by service:**

1. **Route 53** — resolves the domain to CloudFront's IP
2. **WAF** — checks the request isn't malicious, blocks it if it is
3. **CloudFront** — serves the cached React frontend (the button itself)
4. Button click sends an API request -> **ALB**
5. **ALB** — routes the request to a healthy EKS pod
6. **EKS pod (Node.js/FastAPI)** — runs business logic: validates the order, checks stock, checks user auth
7. App checks **ElastiCache Redis** first — is stock/session data cached? If yes, skip DB
8. If not cached — queries **Aurora PostgreSQL** — creates the order record, updates stock
9. If there's an uploaded file (like an invoice/receipt) — stored in **S3**
10. DB credentials the app used to connect — pulled from **Secrets Manager**, not hardcoded
11. **CloudWatch** logs the entire transaction for monitoring/debugging

**Simple summary:** Every single AWS service in your stack has a specific job in this one flow — that's the whole point of the architecture.

**Follow-up questions to expect:**
- "What if the ALB routes to a pod that then crashes mid-request?" (Answer: ALB health checks catch it and stop sending traffic there, but the in-flight request likely fails — this is why idempotent retries matter)
- "Where would you add a circuit breaker in this flow?" (Answer: around the DB call — if DB is slow/down, fail fast instead of hanging the whole request)

---

## How to use this file

- Read one question, cover the answer, try to answer out loud in your own words
- If you can't hit the bullet points from memory, you don't know it yet — re-read and try again tomorrow
- In the actual interview, always trace the **flow first** (which service touches the request in what order) before jumping into details — interviewers want to see you think in systems, not just recite facts
