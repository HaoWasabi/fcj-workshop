---
title: "Blog 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Foundational Knowledge – A Solid Launchpad for Conquering Cloud Computing

Hello everyone!

I'm a fan of Backend and Cloud Computing and I've always believed that learning technology isn't just about knowing how to use tools, but more importantly, understanding the core principles behind them.
During my time learning and practicing with AWS, I realized that my journey in learning Cloud computing was much smoother than I had imagined. It's not because AWS is simple, nor because I can memorize hundreds of services, but because I had already had the opportunity to access fundamental knowledge such as System Design, Networking, APIs, Databases, and system building principles.

This knowledge became the "map" that helped me connect AWS services to real-world problems instead of learning them as isolated concepts.

Through this article, I want to share a personal perspective: foundational knowledge is the solid springboard that helps us learn Cloud computing faster, understand it more deeply, and apply it more effectively.

Hopefully, this article will give you a new perspective if you are starting your journey to conquer cloud computing.
In recent years, cloud computing has become one of the most important skills for programmers, systems engineers, and data engineers. However, when I started learning about AWS, I was overwhelmed by hundreds of services with completely new names: EC2, VPC, IAM, ECS, Lambda, CloudFront, Route 53,...
I also asked myself: "Do I have to memorize all these services?" Ultimately, my answer is no.

What helped me learn AWS faster wasn't my ability to memorize service names, but rather the foundational knowledge I had already acquired: System Design, APIs, Networking, Databases, Operating Systems, and system building principles. Mapping each foundational concept to the use cases of each service made learning and applying it to architecture easier than ever.

## AWS Doesn't Teach You New Concepts

After some time learning and practicing, I realized that most AWS services are implementations of long-standing principles.

- AWS didn't create the concept of servers. They provide EC2.

- AWS didn't invent computer networks. They built VPCs, Route Tables, Internet Gateways, NAT Gateways, Security Groups, and Network ACLs to help us manage networks in the cloud.

- AWS didn't create databases. They provide RDS, Aurora, and DynamoDB to run database management systems in various models. - AWS didn't invent HTTP or APIs. They provide API Gateways, Load Balancers, and many other services to help APIs operate stably at scale.

- This means that if you understand the nature of a traditional system, learning AWS is no longer about learning a completely new technology, but about mapping foundational knowledge to how AWS implements it.

## When you understand the principles, every service makes sense

Before learning AWS, I spent time learning about system architecture, network protocols, and how APIs work. As a result, when I saw a new service, I no longer asked:

"What is this service used for?"

Instead, I asked:

"In a traditional system architecture, what problem needs to be solved? What service is AWS using to solve that problem?"

Just a small change in thinking made the learning process much more intuitive.

Examples:

- Understanding Load Balancers first makes Application Load Balancers no longer a foreign concept.

- Understanding Subnets and Routing first makes VPC a familiar network model.

- Understanding Reverse Proxy makes CloudFront and API Gateway easier to visualize.

- Understanding Docker makes ECS and ECR simply extensions for managing containers on cloud infrastructure.

- Understanding Database Replication makes Multi-AZ and Read Replica less difficult terms to remember.

I don't learn each service independently. I learn by connecting them with my existing knowledge.

## Cloud doesn't replace foundational knowledge

Cloud doesn't replace foundational knowledge; it's just a new environment to apply it.

- An engineer who doesn't understand computer networks will find it very difficult to configure VPCs correctly.

- Someone who has never designed APIs will find it difficult to choose the right architecture between API Gateway, Load Balancer, and Lambda.

- Someone unfamiliar with high availability will find it difficult to understand why Multi-AZ, Auto Scaling, or Load Balancers are necessary.

- Someone unfamiliar with security principles will only know how to grant IAM permissions in a way that allows it to run, instead of adhering to the principle of minimum privileges.

Cloud helps to deploy systems faster, but it cannot replace technical thinking.

## Learning Cloud is learning how to solve problems

What interests me most when learning AWS is not the number of services, but how each service is created to solve a real-world problem.

Every time I learn about a service, I always start with two questions:

- How to solve the problem?

- What did AWS do to simplify or optimize that solution?

Once I answered these two questions, I no longer learned by memorizing names or manipulating the admin interface. Instead, I understood why the service exists, knowing when to use it and when not to. That's the difference between learning a tool and learning a mindset.

## Fundamental knowledge is a long-term investment

Cloud will change. Service names may change. Admin interfaces may change. Some technologies will eventually be replaced.

But the principles of computer networks, operating systems, databases, APIs, security, and system design will remain relevant for many years to come.

That's why I always believe that investing in fundamental knowledge is the most valuable long-term investment for a software engineer. It not only helps you learn AWS faster, but also allows you to approach any other cloud computing platform with the same mindset.

## Conclusion

If you're starting your cloud learning journey and feel overwhelmed by the hundreds of services, my advice is to pause trying to memorize everything. Instead, go back to the fundamentals.

Understand how a system works. Understand why you need networking, APIs, databases, load balancing, security, and system architecture.

Once you've mastered those first building blocks, AWS will no longer be a collection of disconnected services. It will become an ecosystem where each service has a clear place, purpose, and meaning.


## Image


![Example Image](/images/blog3.jpg)


## Link


- [AWS Study Group FCJ](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2233901344041492/)


## References


1. [What is Cloud Computing? - AWS](https://aws.amazon.com/what-is-cloud-computing/)
2. [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/wellarchitected-framework.html)
3. [Amazon EC2](https://docs.aws.amazon.com/ec2/)
4. [Amazon VPC](https://docs.aws.amazon.com/vpc/)
5. [AWS Identity and Access Management (IAM)](https://docs.aws.amazon.com/iam/)
6. [Amazon API Gateway](https://docs.aws.amazon.com/apigateway/)
7. [Amazon RDS](https://docs.aws.amazon.com/rds/)
8. [Amazon DynamoDB](https://docs.aws.amazon.com/dynamodb/)

Ho Chi Minh City, August 04, 2026 <br>
Luong Tuan Giai
