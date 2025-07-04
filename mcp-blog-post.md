---
layout: post
title: "MCP: The New API Layer for AI"
date: 2025-07-04
categories: [technology, api, ai, mcp]
excerpt: "If you've ever spent weeks integrating an AI tool with your API, only to watch it break when you update a single endpoint, you'll understand why MCP caught my attention."
---

# MCP: The New API Layer for AI

If you've ever spent weeks integrating an AI tool with your API, only to watch it break when you update a single endpoint, you'll understand why MCP caught my attention. After implementing a few of these servers myself, I'm convinced this is more than just another API standard—it's a fundamental shift in how we'll build AI-connected systems.

MCP (Model Context Protocol) is being touted as the future of how we'll build & connect systems with AI. It's the "new API layer" in the AI world. Here's my primer based on my experience implementing a few of these...

## Why is MCP interesting at all?

For anyone that has built APIs before, AI or otherwise, MCP is simplifying the integration process in a really interesting way, leveraging cutting-edge OAuth specifications like PKCE and dynamic client registration. What does that mean for users?

- Client integrations with an MCP server are standardized. No ClientID & Secret, no API Key, you add a URL, go through an OAuth Flow, and now your client is holding those credentials and gains access to whatever endpoints you've enabled for it. 
- Clients "discover" tools, prompts, and resources from the MCP server automatically. Add a new tool? Modify a tool? Change the parameters? Doesn't matter, the client will just fetch the latest tools. Think of it like having your API documentation that's also executable—when you ship a new feature, every connected AI instantly knows how to use it.

But here's where it gets really interesting...

## Isn't it just an API though? Don't I already have one of those?

Literally! MCP Servers are just providing APIs to AI clients. They're mostly just wrappers for your apps bespoke (REST/GraphQL/whatever) APIs. **The difference is these APIs are being consumed directly by your customer.** That's a big difference in how you think about designing your APIs. Your consumer isn't a server somewhere, your actual customer is going to see the name of your tools, the parameters you're providing it. 

Traditional API design focuses on efficiency and developer ergonomics. MCP design focuses on human comprehension. Instead of `GET /api/v2/users/{id}/transactions?status=pending`, you're designing tools like "Show me my pending transactions" with natural language descriptions.

## Is an MCP Server just... a well designed API?

Pretty much... It's an API that a human (or human delegated agent) is going to interact with. That means thinking about it really as a product. It's not just like a "good external API" either. You have to really consider how you design it, how someone will interact with it, and "wrap" a lot of that internal logic that doesn't make sense to an end user. 

For example, instead of exposing separate endpoints for authentication, user lookup, and data retrieval, you might create a single tool called "Get my account summary" that handles all the orchestration behind the scenes.

## Do all MCP Servers work the same?

A lot of people are advertising having an MCP Server. In actuality, they've built an MCP wrapper around their API, slapped it into a Docker container, and called it a product that you get to host and update yourself. There's no authentication, no automatic updates, just another piece of software you get to run. This can be useful in certain environments, but I think it misses a lot of the potential associated with MCP Servers. Not to mention, good luck ever changing the APIs you wrapped—now you're maintaining versioning for both your original API and the MCP wrapper.

The real value is what are called "Remote MCP Servers" where someone just hands you a URL and says "give this to your client". That's the magical experience. No installation, no updates, no maintenance. It's like the difference between self-hosting WordPress and using WordPress.com. When I connect to a remote MCP server, I get instant access to the latest tools, and when the provider ships updates, I get them automatically. Not surprisingly, these are far fewer and they're much harder to get right. I've mainly avoided the local ones, focusing on building remote MCP servers. 

## These MCP Servers sound great! What could possibly go wrong?

It's still early, but here's what I'm thinking about and watching in this space:

- **You need to be really thoughtful in how you define your tools.** A tool needs to be scoped to an action that a user might take. While AI can discover how to tie multiple tools together, you can simplify that experience by offering a good wrapper around it. I learned this the hard way—my first MCP server had 47 tools for what should have been 5 user actions.

- **Balancing "power users" and "casual users" is going to be hard.** Your power user might want to be able to make complex queries with filters and aggregations like you have in your external API, but that doesn't map to an average user who just wants to "see last month's expenses." The challenge is you can include all the tools to address all use cases, but you're now making it harder for the AI to determine which tools to provide. I'm imagining a pattern where MCP Servers dynamically modify which tools they have depending not just on entitlements, but the type of user. 

- **Versioning still matters... sort of.** For your "Chat with AI" use case, it's not going to matter much. The clients will discover what the MCP server has on offer at any given time. But for programmatic use cases, both agentic ones and declarative methods like "flows", users are often prompting for specific tools based on past experience. When you rename "Get transactions" to "View transaction history," someone's automated workflow is going to break.

- **The Authentication & Authorization scheme is still nascent.** Technically, the OAuth 2.1 spec in use for MCP Servers is a nightmare to implement right now. Most OAuth Providers don't support all the required features—try getting dynamic client registration working with Auth0 or Okta without pulling your hair out. While there are workarounds, good luck if you work in a big enterprise with strict security requirements. This will mature quickly, but the philosophy around them is still up for grabs. If you already have a strong identity & access practice across your APIs, you'll have a leg up. 

- **We have the same old distributed traceability & serviceability challenges.** Especially for internal systems, proper instrumentation is, as if not more, important as any microservices architecture. When a user says "the AI couldn't complete my request," you need to trace through client → MCP server → your backend APIs → and back.

## Where we go from here

Overall I'm very bullish on the opportunity. A year from now, I think most companies will be providing remote MCP servers as a way to interact with their systems. I think the ecosystem of tools will adapt quickly, but we're going to rediscover a lot of the things we learned designing well-built REST APIs—just through a different lens.

If you're thinking about building an MCP server, my advice is to start with a single, well-designed use case. Focus on the remote server experience from day one. And most importantly, remember you're designing for humans, not machines. That's the real paradigm shift here.

The companies that get this right won't just be wrapping their APIs—they'll be reimagining how their customers interact with their services in an AI-first world. And that's pretty exciting.