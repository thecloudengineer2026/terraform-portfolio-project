# Engineering Decisions & Lessons Learned

## Introduction

This project began as a straightforward website deployment exercise.

The stated goal was simple:

> Deploy a Next.js portfolio website using Terraform, Amazon S3, and CloudFront.

At first glance, this appears to be a task focused primarily on AWS services and Terraform syntax.

However, the most valuable lesson from this project had very little to do with Terraform itself.

Instead, it forced me to rethink how I approach technical problems.

---

# My Initial Approach

When I first reviewed the project requirements, my instinct was to think about AWS services.

Questions immediately came to mind:

* Should I use EC2?
* Should I use Elastic Beanstalk?
* Should I use ECS?
* Should I use Lambda?
* Should I use Amplify?

I was attempting to solve the problem by selecting technologies before fully understanding the problem itself.

In hindsight, this was backwards.

---

# The Shift to First-Principles Thinking

As I worked through the project, I realized I was becoming focused on implementation details rather than business requirements.

Instead of asking:

> Which AWS service should I deploy?

I began asking:

> What capabilities does the system actually require?

This completely changed my approach.

---

# Understanding the Application

The first breakthrough came from understanding what Next.js was producing.

After running:

```bash
npm run build
```

the application generated a collection of static files:

```text
HTML
CSS
JavaScript
Images
Static Assets
```

This observation led to an important realization:

The application no longer required a server.

It simply required a way to store and deliver files.

---

# Reframing the Problem

Once I stopped thinking about AWS services and started thinking about system requirements, the problem became much simpler.

The system only needed to:

1. Store files
2. Deliver files globally
3. Scale automatically
4. Minimize operational overhead
5. Remain cost effective

Everything else was secondary.

---

# Why S3 Was Selected

Amazon S3 satisfied the storage requirement perfectly.

Benefits included:

* High durability
* High availability
* Low cost
* Automatic scalability
* Native static website hosting

Most importantly, it eliminated the need to manage servers.

---

# Why CloudFront Was Selected

The client requirement included global accessibility and fast loading times.

CloudFront provided:

* Global edge caching
* Reduced latency
* HTTPS support
* Improved user experience
* Automatic scaling

Rather than every visitor retrieving content directly from S3, CloudFront caches content closer to users around the world.

---

# Why I Rejected EC2

An EC2 deployment would have worked.

However, it would have introduced additional responsibilities:

* Operating system maintenance
* Security patching
* Monitoring
* Capacity planning
* Scaling considerations

The application simply did not require that level of infrastructure.

This reinforced an important architectural principle:

> The best solution is not the most powerful solution.

> The best solution is the simplest solution that satisfies the requirements.

---

# Terraform Challenges

This project also introduced several real-world troubleshooting scenarios.

## Provider Installation Issues

Terraform initially failed to download the AWS provider.

This required manually downloading the provider and placing it within Terraform's local plugin structure.

The experience reinforced the importance of understanding how Terraform discovers and installs providers.

---

## Backend Configuration Errors

While configuring the S3 backend, initialization failed because of syntax issues within the backend block.

Although the error itself was simple, it highlighted how Terraform validates configuration before initialization.

---

## State Locking Problems

Terraform later failed while attempting to acquire a lock through DynamoDB.

The issue was ultimately related to backend configuration and state locking behavior.

This experience provided valuable insight into how Terraform protects state consistency in collaborative environments.

---

## S3 Public Access Controls

One of the most interesting issues occurred when Terraform attempted to apply the bucket policy.

AWS rejected the operation because S3 Block Public Access settings prevented public policies from being attached.

This demonstrated an important cloud engineering lesson:

Infrastructure failures are not always caused by code.

Many failures occur because security controls intentionally prevent potentially risky configurations.

---

# Git and GitHub Lessons

Prior to this project, I had limited experience using Git as part of a complete engineering workflow.

This project reinforced several key concepts:

* Repository structure matters
* Version control should be integrated from the beginning
* Infrastructure code should be treated like application code
* Documentation is part of the deliverable

The final repository contains both application code and infrastructure code, reflecting a more realistic engineering workflow.

---

# The Most Valuable Lesson

The biggest lesson from this project was learning to think in terms of systems rather than technologies.

Early in my cloud journey, I often approached problems by asking:

> Which AWS service should I use?

Today I find myself asking:

> What problem am I trying to solve?

Once the requirements are clearly understood, the architecture often becomes much simpler.

This project reinforced the value of:

* First-principles thinking
* Systems analysis
* Requirements-driven architecture
* Simplicity over complexity

Those lessons will continue to be valuable long after the Terraform syntax itself is forgotten.

---

# Future Improvements

Potential future enhancements include:

* Route53 custom domain integration
* ACM SSL certificate management
* CI/CD deployment pipeline using GitHub Actions
* Terraform modules for reusable infrastructure
* Multi-environment deployments
* Automated cache invalidation
* Monitoring and alerting

These enhancements would move the project closer to a production-ready deployment architecture.

---

# Final Reflection

What began as a website deployment exercise ultimately became a lesson in cloud architecture and systems thinking.

The technical implementation was important, but the larger takeaway was learning to identify requirements first, architecture second, and services last.

That shift in thinking fundamentally changed how I approach engineering problems.
