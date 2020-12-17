## Answers to your common DNS CNAME issues

DNS issues can get really frustrating. You have a new shiny website/webapp that you want served up to your visitors. You also have a new domain which must be pointed at the CDN (or Load Balancer) of your host. While googling, you find some nice instructions for setting the DNS records. So you head to your DNS configuration, where you successfully set the CNAME record to point to your target. Now all you need to do is wait for TTL; for your new records to get propagated right? You check every hour or so for the next 24 hours, but your website is still not serving up. Wait! There must be something wrong. Is the host server down? Well, probably not. It has 99.999999999% availability. 
> If this already sounds like you, take a deep breath. Remember, we have all been there; grab a cup of coffee or tea, and let's learn some more about CNAMEs. 

# What at all is a CNAME?  
In simple terms, a CNAME record points a name to another name. It is used, for instance, when you want to point visitors of your domain name `app.example.com` to another name such as `app.something.com`. Below you see how I pointed my blog url `blog.nquayson.com` at hashnode's network url. It is that simple.  
  

![cname.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1608086466474/LizEIxNJR.png)
  
  

# Why are CNAME records getting so popular?
The rise in cloud computing is a factor here. A common way to point a domain to a CDN is to add the address as a CNAME. Also, many providers use CNAME records as a way to validate domain ownership. 


# The common issues:

- CNAME record cannot be at the Apex/root domain level. 
- CNAME record cannot be defined with other record types (eg MX, TXT, A) for the same name. 
- MX record cannot point to CNAME
- CNAME record CAN point to other CNAME records, however this practice is considered inefficient. 


# Why not directly point at IPs rather than names?
To ensure high availability, many modern hosting providers and CDNs generally run on distributed system architectures. When host node instances fail, or when there is a better server node available (based on factors such as latency, proximity, health, or cost) providers may have to route traffic to a different node. This will result in a change of destination IP. So your application will break, if you pointed your service directly to an IP.  

Some providers will allow creating one CNAME at the domain apex, because of CNAME flattening, but then that can cause other records such as TXT, MX to not work properly. 
Let's say your domain name is `www.example.com`
We refer to `example.com` as the 'apex' or 'naked' or 'root'. Some providers such as namecheap use the '@' symbol to represent the apex. The problem with the apex domain is that setting a CNAME record, breaks any other records associated with this name. For instance, you might want to use email service on your domain. However, by setting a CNAME for the apex, you will not be able to use email. 


# Are there any workarounds? 

Yes: CNAME flattening or simply using www. First, you may have heard of the ALIAS record type (not to be confused with the 'A' record which points name to IP address). 
>The ALIAS record (also referred to as an ANAME) is not traditionally part of 
the DNS spec. They are virtual record types usually found with cloud DNS providers for their own internal use.  

The way it works is, when the authoritative name server receives a request for a record which has an associated ALIAS record, it resolves the ALIAS first. Let's say the ALIAS points to another CNAME. It will then resolve that CNAME down to it's A records. 
They work pretty like CNAME record type but are a way to solve some of the many problems that are associated with the CNAME record. 

# What if your DNS provider does not support the ALIAS record

Rather than set the CNAME to your website at the domain apex, you could set it on a subdomain such as www. Keeping www as your main URL or canonical domain frees up the apex, as well as other subdomains to be used for other services such as email. 

Let me know in the comments below whether you have faced any CNAME issues in the past, and how you resolved it! 