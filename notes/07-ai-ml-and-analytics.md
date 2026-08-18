# AI / Machine Learning & Analytics Services

Notes covering Amazon Bedrock, AWS SageMaker, Amazon Rekognition, Amazon Polly, Amazon Transcribe, Amazon EMR, and Amazon Athena.

---

## 1. <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/ArtificialIntelligence/Bedrock.png" width="40" height="40" valign="middle" /> Amazon Bedrock
- **Category**: Generative AI Platform
- **Core Purpose**: Managed service for building generative AI applications using leading Foundation Models (FMs).

### Key Concepts
- **Foundational Models**: Provides access to third-party and Amazon LLMs (Anthropic Claude, Meta Llama, Stability AI).
- **Developer Focused**: Targeted at application developers creating chatbots, text generators, image generation, and voice workflows without managing AI infrastructure.

---

## 2. <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/ArtificialIntelligence/SageMakerAI.png" width="40" height="40" valign="middle" /> AWS SageMaker
- **Category**: Machine Learning Platform for Data Scientists
- **Core Purpose**: End-to-end platform for building, training, tuning, and deploying custom ML models.

### Key Concepts
- **ML Lifecycle**: Built for data scientists to manage data preparation, notebook training, model deployment endpoints, and drift monitoring.

---

## 3. <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/ArtificialIntelligence/Rekognition.png" width="40" height="40" valign="middle" /> Amazon Rekognition
- **Category**: Computer Vision AI
- **Core Purpose**: Automated image and video analysis.

### Key Concepts
- **Computer Vision Capabilities**: Detects faces, facial expressions/emotions, text inside images, object scenes, and inappropriate content.

---

## 4. <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/ArtificialIntelligence/Polly.png" width="40" height="40" valign="middle" /> Amazon Polly
- **Category**: Text-to-Speech (TTS)
- **Core Purpose**: Converts plain text into spoken lifelike audio output.

### Key Concepts
- **Neural Speech Synthesis**: Customizable voices and speech marks for accessibility and voiceover automation.

---

## 5. <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/ArtificialIntelligence/Transcribe.png" width="40" height="40" valign="middle" /> Amazon Transcribe
- **Category**: Speech-to-Text Transcription
- **Core Purpose**: Converts audio files or streams into accurate text transcripts.

### Key Concepts
- **Speaker Diarization**: Distinguishes between multiple speakers during medical appointments, customer service calls, or interview recordings.

---

## 6. <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/Analytics/EMR.png" width="40" height="40" valign="middle" /> Amazon EMR (Elastic MapReduce)
- **Category**: Big Data Cluster Processing
- **Core Purpose**: Managed big data processing using Apache Spark, Hadoop, Presto, and Iceberg.

### Key Concepts
- **Big Data Clusters**: Provisions processing clusters to parse petabytes of raw data stored in S3 and output transformed analytical datasets.

---

## 7. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-athena.svg" width="40" height="40" valign="middle" /> Amazon Athena
- **Category**: Serverless Interactive SQL Analytics
- **Core Purpose**: Runs interactive SQL queries directly against raw files stored in Amazon S3 buckets.

### Key Concepts
- **Serverless Analytics**: No clusters to provision or manage; pay strictly per query based on the volume of S3 data scanned.
- **S3 Data Lake Querying**: Directly queries JSON, CSV, Parquet, or ORC files in place.
