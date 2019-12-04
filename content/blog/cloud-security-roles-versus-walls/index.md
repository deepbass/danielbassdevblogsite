---
title: Secure Serverless 
date: "2019-11-20T09:00:00.000Z"
description: High level introduction to being secure with Serverless
---
Cloud security is a big topic at the moment, with many organizations trying to port their on-prem models over to the cloud and finding it doesn't work. The price of finding this out can be absolutely brutal, with user data leaked across the internet and your reputation ruined. 

So how do you avoid this? Luckily OWASP have some good advice on the principles of security by design when building new software [here](https://www.owasp.org/index.php/Security_by_Design_Principles). I'm not going to go through every principle in detail but there's some key things I'd like to extract:
* Minimize the Surface Area. Basically leave as little as possible vulnerable to attack, and its less likely it will be successfully attacked
* Defence in Depth. Try and have multiple interlocking security measures, so that if one is breached other's kick in and save you.
* Least Privilege. Each component should only have the permission required to do its job and nothing else.
These three principles form the basis of most of the modern approach to security. You'll probably have seen them in action - you are likely an admin of your home computer, but at work your IT department will have exercised the least privilege principle and reduced your powers to what you need to do your job. This limits the downside of your account being hacked - if you had god rights and got hacked, your account would be a colossal danger to the company. 
## How does Serverless fit in? 
Let's kick off with what Serverless helps with on the security front. 
* Serverless applications have a massively minimized Surface Area in terms of vulnerabilities compared to traditional architectures (i.e Bare Metal, Virtual Machines or Containers). A serverless function has no operating system to attack, with the entire responsibility for security of the platform outsourced to the provider. I didn't even notice when Heartbleed and the various Intel vulnerabilities were published, my cloud provider patched them seamlessly. Compare this to even containers, the next best option, and you see the 10 most popular Docker Images on DockerHub are riddled with security vulnerabilities. Let's not even get into the vulnerabilities of a windows server running in a cupboard somewhere. 
* Serverless architectures actively encourage the use of role based access, with each function having a tiny set of permissions to just do it's job effectively.
* The use of Managed Services to solve problems is actively encouraged. These managed services notably include login systems, which are developed and maintained by some of the best engineers in the world - these are significantly more secure than individual developers creating their own login systems and making massive errors like storing passwords in plaintext. 
* Defence in depth is strong, but hidden. Underlying a serverless product are many layers of defence, but because they are abstracted it sometimes looks like they aren't there. For example a serverless database will have many layers of physical security protexting the servers, layers of software-based protection and active monitoring for malicious activity. This is more than most organizations could ever hope to achieve, but it looks like only one box on an architecture diagram, rather than 27. 

This all sounds good, and I truly believe Serverless Architectures are some of the most secure available. There are some issues though: 
* Serverless functions encourage lots of small components that are each possible targets. This could be said to increase surface area (although arguably this is cancelled out by the fact that each of these has a much lower permission set than a traditional system which would have only a few components but each of them having a much wider permission set)
* Serverless tends to not play nicely with firewalls and network restrictions. This is because to scale elastically they need to allocate network addresses dynamically and this is very challenging to do inside a private network. This makes it impossible/very hard to use something that increases the Defence in Depth. 

## Are those issues addressable?
Yes, but not quite at the moment. AWS has recently taken a leap forward in being able to provide Lambda's inside firewalls/private networks with a reasonable cold start time. The trouble is the problem is challenging for each service, and I'd question whether you are exposing yourself to more risk by refusing to use the service until the networking issues are resolved. If you decide to use Kubernetes with Docker instead, you are exposing yourself to far more risk than a Serverless function which has a completely managed runtime