---
title: "Event 2"
date: "2026-06-06"
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Summary Report: "First Cloud Journey Meetup - Cloud, DevOps, AI, and Career Development"

### Event Objectives

- Share practical knowledge of cloud computing, DevOps, cybersecurity, AI, and software development
- Connect foundational concepts with AWS architectures and deployment use cases
- Help attendees better understand teamwork, self-learning, and career development in the IT industry
- Create opportunities for speakers and attendees to network and exchange experience

### Highlights from Each Session

#### Session 1 | Docker: A Containerization Technology

**Main content:**

- Introduction to virtualization and benefits such as isolation, encapsulation, and portability
- Analysis of Virtual Machine limitations: each VM has its own operating system, consumes substantial CPU, memory, and storage, requires independent updates, and is relatively heavy for small applications
- Explanation of containerization as a way to package an application with its dependencies and configurations so it runs consistently across environments
- Comparison of Virtual Machines and Containers in terms of architecture, startup speed, resource consumption, and deployment capability
- Introduction to Docker based on the principle of “build once, run anywhere”
- Explanation of Docker Images, Docker Containers, Dockerfiles, image layers, and build cache
- Overview of Docker command groups and a practical demonstration
- Use cases including CI/CD, microservices, development and testing environments, cloud-native applications, and legacy application modernization

**Key takeaway:**

I gained a clearer understanding of the difference between virtualization and containerization. Containers do not replace VMs in every situation, but they provide a lightweight, consistent, and highly portable deployment unit. A Dockerfile also turns environment configuration into code that can be version-controlled, reproduced, and integrated into a CI/CD pipeline.

#### Session 2 | Combining AWS WAF with Machine Learning for Cyber Attack Detection on AWS

**Main content:**

- Overview of AWS WAF and its ability to protect CloudFront, Application Load Balancer, API Gateway, and Cognito against SQL injection, XSS, bot traffic, brute-force attacks, and abnormal HTTP/HTTPS requests
- Analysis of the limitations of rule-based and signature-based mechanisms when facing zero-day attacks, hybrid attacks, and previously unseen behavior
- Introduction to Network Intrusion Detection Systems (NIDS) for monitoring traffic, analyzing behavior, detecting threats, issuing alerts, and recording events
- Explanation of how Machine Learning learns from network behavior and identifies new attack patterns
- Use of the CSE-CIC-IDS2018 dataset with labels including benign, bot, DoS, brute force, XSS, SQL injection, FTP/SSH brute force, and DDoS
- Data preprocessing: merging CSV files, cleaning labels, processing negative values, NaN, and infinity, removing unnecessary columns, balancing classes, and splitting training and test sets
- Evaluation of a LightGBM model with a confusion matrix and improvement of detection for minority attack classes
- AWS architecture comprising Amazon EC2, Application Load Balancer, AWS WAF, Amazon S3, Kinesis Data Firehose, AWS Lambda, Security Hub, GuardDuty, Inspector, SNS, IAM, AWS Config, and CloudWatch
- Development of a real-time attack monitoring dashboard that correlates NIDS predictions with AWS WAF events
- Future improvements: ingesting real-world data streams, integrating Amazon Bedrock, and automating incident response

**Key takeaway:**

I recognized that AWS WAF and an ML-based NIDS should be treated as complementary protection layers. WAF handles known attack patterns effectively, while Machine Learning improves the detection of abnormal behavior. The effectiveness of an ML system depends heavily on data quality, class balance, real-time monitoring, and continuous model updates.

#### Session 3 | Multiplayer in the Cloud: Connecting Godot Clients with AWS WebSockets

**Main content:**

- Comparison of UDP/ENet, WebSocket, and HTTP Polling by latency, reliability, connection cost, and suitable game types
- Selection of WebSocket for turn-based games, lobbies, and chat because it supports full-duplex communication and reliable delivery
- Architecture using Godot Client → Amazon API Gateway WebSocket → AWS Lambda → Amazon DynamoDB, with Amazon CloudWatch for logging and monitoring
- Use of `$connect`, `$disconnect`, `$default`, and custom routes with the route selection expression `$request.body.action`
- DynamoDB item design using `connectionId` as the partition key, with `status`, `opponentId`, `choice`, and `createdAt` attributes
- Lambda logic for connection, disconnection, opponent discovery, matchmaking, receiving choices, and returning results
- Godot 4 integration using `WebSocketPeer`, `connect_to_url()`, `poll()`, `send_text()`, and a packet-receiving loop
- Demonstration of two clients connecting, creating DynamoDB records, matching with each other, submitting Rock/Paper/Scissors choices, and receiving results simultaneously
- Analysis of challenges including stale connections and `GoneException`, the cost of DynamoDB Scan, and the stateless nature of Lambda
- Comparison of serverless WebSocket + Lambda with a dedicated server on AWS GameLift for games requiring high-frequency updates and authoritative in-memory state

**Key takeaway:**

I learned that multiplayer architecture must begin with gameplay requirements. Serverless WebSocket is suitable for turn-based games and lobbies, but it is not optimized for continuous physics synchronization in FPS or racing games. Beyond connectivity, a production system must manage the connection lifecycle, optimize DynamoDB queries, handle failures, and control costs at scale.

#### Session 4 | The Art of Effective Teamwork

**Main content:**

- Distinction between individual productivity and overall team performance
- Four core principles of effective teamwork:
  1. Clear & Shared Goals — goals must be clear and understood by the whole team
  2. Right Person, Right Place — assignments should match each member's abilities and strengths
  3. Open Communication & Active Listening — communicate openly and listen actively
  4. Personal Accountability — every member takes responsibility for their tasks and commitments
- Introduction to collaboration and task-management tools such as ClickUp, Trello, Slack, Google Workspace, and Discord
- Illustration of how Discord can organize channels, support content exchange, and improve team coordination

**Key takeaway:**

I recognized that tools only support the process; the foundation of teamwork remains shared goals, appropriate task allocation, transparent communication, and personal accountability. An effective team must agree on how tools are used, communication rules, and standards of completion instead of merely creating more channels or tasks.

#### Session 5 | AWS Neptune for Building a Graph Knowledge Base for GraphRAG

**Main content:**

- Review of Retrieval-Augmented Generation (RAG): retrieving relevant passages from a knowledge base and inserting them into a prompt so an LLM can generate a context-grounded answer
- Analysis of the limitations of vector RAG for questions requiring multi-hop reasoning across many entities and documents
- Introduction to GraphRAG and its two main advantages: graph traversal for multi-hop reasoning and explicit relationship storage through edges
- Presentation of a fully managed route using Amazon Bedrock Knowledge Bases and Amazon Neptune Analytics
- Amazon Bedrock Knowledge Bases handles chunking, entity extraction, and embedding generation, while Neptune Analytics stores graph data as nodes and edges and supports relationship discovery
- Presentation of a custom route using LlamaIndex to prepare data and build the knowledge graph, combined with Amazon Neptune for graph storage, multi-hop traversal, and Cypher queries
- Comparison of managed GraphRAG and open-source GraphRAG in operational effort, customizability, deployment speed, and pipeline control

**Key takeaway:**

I learned that GraphRAG is especially useful when a question depends on relationships among multiple entities rather than merely finding semantically similar passages. The fully managed route is suitable when scalability and lower operational complexity are priorities, while the custom route is appropriate when deeper control over graph construction, retrieval logic, and query behavior is required.

#### Session 6 | From IT Helpdesk to Senior Sysadmin: Self-learning Journey and Transition to Cloud/DevOps

**Main content:**

- A real career journey that began in IT Helpdesk and progressed through self-learning and hands-on experience without special advantages
- Foundational Helpdesk skills: troubleshooting under pressure, communicating with end users, developing a problem-solving mindset, and understanding how IT systems operate
- Transition to Sysadmin through learning Linux and Networking, building labs, and exploring infrastructure technologies
- System Administrator responsibilities: server provisioning, network management, security patching, capacity planning, and monitoring
- Operational lessons: automate repetitive tasks, create documentation and runbooks, establish monitoring before incidents occur, collaborate with development teams, and avoid uncontrolled experiments in production
- Transition from on-premises infrastructure to a cloud mindset with AWS, elastic scaling, pay-as-you-go pricing, and managed services
- Introduction to Infrastructure as Code with Terraform, version control, and repeatable deployments, followed by CI/CD, Docker, automation, and DevOps culture
- A modern DevOps roadmap covering Linux & Networking, Git, cloud fundamentals, Docker & Containers, CI/CD, Terraform, Kubernetes, and Monitoring & Observability
- Interview advice: research the company and focus on real projects, architecture design, incident response, and troubleshooting
- Career advice: do not study too many topics simultaneously; develop one or two core strengths in depth, practice through real projects, and build a portfolio

**Key takeaway:**

I learned that a person's starting point does not determine their career limits. Practical operational ability, incident-response thinking, documentation, automation, and a portfolio can support the transition from Helpdesk to Sysadmin, Cloud, and DevOps. An effective roadmap requires clear priorities reinforced through hands-on projects rather than certificates alone.

### Key Lessons Learned

- **Choose architecture according to the problem:** Containers, WebSocket, GameLift, vector RAG, and GraphRAG each have their own suitable scope; no single technology is optimal for every use case.
- **Automation and reproducibility are foundational:** Dockerfiles, Infrastructure as Code, CI/CD, and serverless workflows reduce manual work and improve consistency.
- **Security requires multiple layers:** Rule-based protection should be combined with behavioral detection, monitoring, and automated response.
- **Data determines AI/ML quality:** Data cleaning, class balancing, and model updates directly affect attack-detection effectiveness.
- **Operations must be designed from the beginning:** Logging, monitoring, connection lifecycle, failure handling, documentation, and cost control should not be deferred to the final stage.
- **Human skills remain decisive:** Shared goals, communication, personal accountability, self-learning, and hands-on experience are foundations for sustainable teams and careers.

### Application to Study and Work

- Containerize a small application with a Dockerfile and integrate the image build into a CI/CD pipeline
- Design a proof of concept using API Gateway WebSocket, Lambda, and DynamoDB for a turn-based real-time application
- Explore how WAF logs, network data, and an ML model can be combined in an alerting dashboard
- Compare vector RAG and GraphRAG using the same document set containing relationships among multiple entities
- Apply the four teamwork principles to task allocation, communication, and group progress reviews
- Build a phased Cloud/DevOps roadmap prioritizing Linux, Networking, Git, Docker, AWS, IaC, CI/CD, and monitoring

### Personal Contribution

As an attendee, I actively organized the knowledge from all six sessions into cloud-native, security, real-time application, AI, teamwork, and career-development categories. Comparing architectural options and recording actionable lessons helped me convert the presentations into a concrete learning direction while creating a foundation for further knowledge exchange with my team and the community.

### Event Experience

The First Cloud Journey Meetup on June 6, 2026, offered a multidimensional perspective ranging from foundational topics such as Docker to advanced architectures such as ML-based NIDS and GraphRAG. Its greatest value was the connection among technology, operations, people, and career direction. I learned that developing in Cloud/DevOps requires not only studying AWS services but also cultivating systems thinking, self-learning, collaboration, and practical problem-solving skills.

#### Event Photos
![Event photo](/images/4-EventParticipated/event2.1.jpg)
![Event photo](/images/4-EventParticipated/event2.2.jpg)

> Overall, the event helped me connect technical knowledge with workplace skills and career development. The sessions not only broadened my understanding of AWS but also helped me identify the capabilities I should continue building to pursue a career in Cloud/DevOps.
