# Services of AWS (not complete)
This document contains a "10.000 feet" overview with a simple one-sentence description of the services of AWS.
Only the ones, which I am interested in as well as the ones which I didn't know are listed below.
The subsection description is the heading description in AWS itself, if you are clicking on *Services*.

## Management Tools
CloudTrail = Tracing of actions
Config = Monitors the config of the whole AWS environments, change log, visualiziation of the environments
OpsWorks = Shift/Puppet for provisioning of environments
Service Catalog = IT Services approved on AWS, typically used for compliance/access
Systems Manager = Resource management (patch maintenance), grouping resources by departements
Trust Advisor = Security aspect management (e.g. open ports), safe money with unused resources

## Machine Learning
SageMaker = Deep Learning
Comprehend = Sentiment Analysis
Deep Lens = Artificial aware camera, physical piece of hardware
Lex = Powers the Alexa servers; chatting with customers
Machine Learning: Basically throw a dataset, analyze, predict outcome on new data
Polly = Text to Speech
Rekognition = Video+Images, Content detection

## Analytics
Athena: Analyse via SQL S3 buckets
EMR: Elastic Map Reduce, Big data... 
CloudSearch/Elastic Search: Searching
Kinesis: Ingesting large amounts of data, e.g. tweets, social media
QuickSight: Business Intelligence
Data Pipeline: Moving data between different AWS Services
Glue: ETL 

## Security
IAM = Identiy Access Management
Cognito = Device Authentication, e.g. mobile apps
Inspector = Agent on EC2, run tests against it for security vulnerabilities, report with severities
Macie = Scan S3 buckets after personal information
Certificate Manager =
CloudHSM = Hardware Security Module, dedicated bits of hardware
Directory Service = Microsoft AD integration
WAF = Firewall, stops XSS, SQL Injection etc.

## Mobile
Mobile Hub = Mgmt Console for mobile apps, setup AWS services for mobile
Pinpoint = Push notifications

## AR/VR
Sumerian = Common set of tools, AR/VR/3D Application Design

## Application Integration
Step Functions = 

Exam related
SQS = Queue 
SNS = notification services
SWF = Simple Workflow Service

## Customer Engagement
Connect = Contact Center as a Service
Simple Email Service

## Business Productivity
Chime = Video Conferencing like Hangouts
Work Docs = like DropBox, safely + securely
Work Mail = Comparable to Office365

## Desktop & App Streaming:
Workspaces = Stream OS to mobile devices
