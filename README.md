# Load-Balancer-Auto-Scaling

making applications actually resilient — using a Load Balancer and Auto Scaling Group together.

# The Problem It Solves:
A single server is a single point of failure — if it crashes or gets overloaded, the app goes down. A Load Balancer spreads traffic across multiple servers, and an Auto Scaling Group keeps replacing unhealthy servers automatically — so the app stays up even when individual instances fail.

# The Build:
• Launched two EC2 web servers with a startup script (via User Data) to auto-install Apache and display each server's hostname.

• Created a Target Group pointing to both servers on port 80.

• Set up a dedicated Security Group for the Load Balancer, allowing inbound HTTP from anywhere.

• Created an Application Load Balancer (ALB) across multiple Availability Zones, linked to the Target Group.

• Tested it — refreshing the ALB's DNS name showed traffic switching between both servers' hostnames, confirming live load distribution.

• Built a Launch Template from a working server, then created an Auto Scaling Group (min 1, desired 2, max 3) tied to the same Target Group, with ELB health checks enabled.

• Manually terminated one server to test failover — the ASG detected the deficit and automatically spun up a new instance to restore the desired count.

# Key Takeaways:
• Load Balancer ≠ just traffic routing — it's the entry point that makes horizontal scaling possible.

• ELB health checks matter — they replace servers based on actual app health, not just whether the instance is "running."

• Auto Scaling Groups don't just scale up — they also self-heal by replacing failed instances automatically.

• Watching a terminated server get auto-replaced live made high availability finally click — it's not a theory, it's automatic recovery in action.


