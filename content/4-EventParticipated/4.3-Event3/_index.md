---
title: "Event 3"
date: 2026-06-13
weight: 3
chapter: false
pre: "<b> 4.3. </b>"
---

# Summary Report: "First Cloud Journey Meetup - Career, DevOps, and AWS Architecture"

### Event Objectives

- Gain practical insights into careers and workplace culture in technology companies
- Better understand the role, foundational knowledge, and mindset of a DevOps Engineer
- Explore the development path from the First Cloud Journey community to an AWS Partner environment
- Learn about a scalable URL-shortening service architecture on AWS

### Main Content by Session

#### Session 1 | A Real-World Story: Working Culture at a Multinational Corporation

**Main content:**

- Practical responsibilities of a Data Analytics Engineer: creating reports, designing dashboards, performing root-cause analysis, and supporting decision-making
- Four important capabilities: critical thinking, communication, data storytelling, and problem-solving
- A development path from Follower and Learner to Problem Solver, System Thinker, and Leader
- A common multinational recruitment process: screening, capability assessment, technical interviews, and cultural-fit evaluation
- Values such as No-Blame Post-Mortems, a Caring & Inclusive environment, and working according to global standards

**Key takeaway:**

I learned that analytical capability goes beyond producing reports; it also involves identifying root causes, communicating insights, and recommending solutions. For long-term growth, technology professionals need to move from a task-completion mindset toward problem-solving and system optimization.

#### Session 2 | What Does a DevOps Engineer Really Do?

**Main content:**

- Clarification of common misconceptions that define DevOps solely as CI/CD, Docker, Kubernetes, cloud, or production incident handling
- Explanation of how the DevOps role varies with company size, product type, team structure, and infrastructure maturity
- A foundation-first learning path: Linux, networking, Python or Golang, Git, CI/CD, and containers
- Encouragement to build small projects that practice deployment, automation, monitoring, troubleshooting, and system recovery
- Emphasis on understanding principles rather than copying commands, identifying the correct problem owner, asking “why,” and communicating clearly

**Key takeaway:**

I recognized that DevOps is not a fixed list of tools but a systems mindset that helps teams deliver and operate software reliably. Tools may change, but foundational knowledge, self-learning, automation, and communication remain core capabilities.

#### Session 3 | From First Cloud AI Journey to AWS Partner

**Main content:**

- A journey from student curiosity to First Cloud Journey, community workshops, hands-on labs, and university projects
- The role of a portfolio in demonstrating capabilities and connecting knowledge with real-world problems
- Introduction to the First Cloud AI Journey Program, AWS Student Builder Group Program, and AWS Community Builder Program
- Opportunities to participate in events, build communities, earn badges, and develop leadership ability
- Career opportunities at AWS Partners and the “share back” spirit of supporting the next generation

**Key takeaway:**

I learned that an effective cloud journey combines study, practice, projects, portfolio development, and community contribution. Securing a job is only the beginning; lasting value comes from solving real problems and sharing knowledge with others.

#### Session 4 | A Scalable URL Shortening Service on AWS

**Main content:**

- Explanation of the basic URL-shortening workflow and the limitations of a simple architecture when traffic grows
- A frontend using Amazon Route 53, Amazon CloudFront, AWS WAF, and AWS Amplify
- A backend using Amazon ECS/AWS Fargate, Application Load Balancer, Amazon ElastiCache for Redis, and Amazon DynamoDB in a multi-Availability Zone architecture
- A Key Generation Service that pre-generates short codes and places them in a Redis queue, enabling fast URL-creation responses and reducing code-collision risk
- The cache-aside pattern: read from Redis first and query DynamoDB only on a cache miss
- Emphasis on separation of concerns, defense at the edge, pre-computation, and independent optimization of read and write paths

**Key takeaway:**

I learned that a scalable system must be designed according to its traffic characteristics. Separating read and write paths, pre-generating codes, using caching, and moving the security layer closer to users can reduce latency, limit bottlenecks, and protect the core system more effectively.

### Key Lessons Learned

- Foundational knowledge and problem-solving thinking have more lasting value than memorizing tools
- Data should be transformed into insights and actions instead of ending with reports
- DevOps combines systems, automation, operations, and team communication
- Portfolios and community activities turn learning into demonstrable capabilities
- Cloud architecture must balance performance, scalability, security, cost, and operability

### Application to Study and Work

- Build a small project with CI/CD, containers, monitoring, and operational documentation
- Practice presenting data-analysis results using a problem–cause–solution structure
- Improve my portfolio with AWS labs and projects that have clear architectures
- Design a proof of concept for a URL shortener using Redis and DynamoDB
- Participate actively in community activities and share what I have learned

### Personal Contribution

As an attendee, I actively recorded and organized the content of the four sessions into three areas: career development, DevOps thinking, and AWS architecture design. Connecting the lessons with my learning plan helped me identify priority skills and ways to turn knowledge into practical projects.

### Event Experience

The meetup on June 13, 2026, provided a balanced perspective on technology and career development. Alongside DevOps and AWS architecture knowledge, discussions of analytical thinking, corporate culture, portfolios, and communities helped me better understand how to develop my capabilities sustainably.

#### Event Photos
![Event photo](/images/4-EventParticipated/event3.1.jpg)
![Event photo](/images/4-EventParticipated/event3.2.jpg)

> Overall, the event helped me connect technical expertise with a professional mindset and the spirit of community contribution. This is an important foundation for continued growth in Cloud/DevOps and for building solutions with real-world applicability.
