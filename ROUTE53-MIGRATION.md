# 🔄 Route 53 Nameserver Migration Guide

## ✅ Current Status

Route 53 hosted zone has been created and configured for **urlstatuschecker.com**

### Route 53 Nameservers (Use These in IONOS)

```
ns-669.awsdns-19.net
ns-234.awsdns-29.com
ns-1798.awsdns-32.co.uk
ns-1448.awsdns-53.org
```

### Hosted Zone Details
- **Zone ID**: Z0400631USJUHW2VUTMO
- **Region**: Global (Route 53 is a global service)
- **Status**: Active

## 📋 DNS Records Already Configured

| Type | Name | Value | Purpose |
|------|------|-------|---------|
| A | urlstatuschecker.com | d3fu5iavm1zuit.cloudfront.net (alias) | Main site |
| A | www.urlstatuschecker.com | d3fu5iavm1zuit.cloudfront.net (alias) | WWW redirect |
| CNAME | api.urlstatuschecker.com | omo54416yb.execute-api.us-east-1.amazonaws.com | API endpoint |

## 🚀 Migration Steps

### Step 1: Login to IONOS Dashboard
1. Go to [https://www.ionos.com/](https://www.ionos.com/)
2. Login with your account credentials
3. Navigate to **Domains** → **Domain Management**

### Step 2: Locate urlstatuschecker.com
1. Find `urlstatuschecker.com` in your domain list
2. Click **Manage** or the gear icon

### Step 3: Change Nameservers
1. Look for **Nameserver Settings** or **DNS Settings**
2. Select **Use Custom Nameservers** or **External Nameservers**
3. Replace existing nameservers with Route 53 nameservers:

```
Nameserver 1: ns-669.awsdns-19.net
Nameserver 2: ns-234.awsdns-29.com
Nameserver 3: ns-1798.awsdns-32.co.uk
Nameserver 4: ns-1448.awsdns-53.org
```

4. **Save Changes**

### Step 4: Wait for Propagation
- **Initial Propagation**: 1-2 hours
- **Full Propagation**: 24-48 hours
- **TTL Consideration**: Depends on your current IONOS TTL settings

## ✅ Verification

### Check Nameserver Propagation

```bash
# Check current nameservers
dig NS urlstatuschecker.com

# Check from multiple locations
nslookup -type=NS urlstatuschecker.com 8.8.8.8
nslookup -type=NS urlstatuschecker.com 1.1.1.1

# Check A record resolution
dig urlstatuschecker.com

# Check www subdomain
dig www.urlstatuschecker.com

# Check API subdomain
dig api.urlstatuschecker.com
```

### Online Tools
- [DNS Checker](https://dnschecker.org/)
- [What's My DNS](https://www.whatsmydns.net/)
- [MX Toolbox](https://mxtoolbox.com/SuperTool.aspx)

### Expected Results After Migration

```bash
$ dig NS urlstatuschecker.com

;; ANSWER SECTION:
urlstatuschecker.com.  172800  IN  NS  ns-669.awsdns-19.net.
urlstatuschecker.com.  172800  IN  NS  ns-234.awsdns-29.com.
urlstatuschecker.com.  172800  IN  NS  ns-1798.awsdns-32.co.uk.
urlstatuschecker.com.  172800  IN  NS  ns-1448.awsdns-53.org.
```

## 🎯 Benefits of Route 53

### 1. **100% Uptime SLA**
   - AWS guarantees 100% uptime for Route 53
   - Distributed globally across AWS edge locations
   - Automatic failover and health checking

### 2. **Performance**
   - Anycast routing for fastest DNS resolution
   - Sub-millisecond query response times
   - Automatic latency-based routing

### 3. **Easy Management**
   - AWS Console, CLI, and API access
   - Integrated with CloudFront, S3, Lambda
   - Programmatic subdomain creation

### 4. **Advanced Features**
   - Health checks and monitoring
   - Traffic flow policies
   - Geolocation routing
   - Weighted routing for A/B testing
   - Alias records (no additional cost)

### 5. **Cost-Effective**
   - $0.50/month per hosted zone
   - $0.40 per million queries
   - No charge for alias queries to AWS resources

## 🔧 Adding Subdomains (After Migration)

### Via AWS CLI
```bash
# Add dns.urlstatuschecker.com subdomain
aws route53 change-resource-record-sets \
  --hosted-zone-id Z0400631USJUHW2VUTMO \
  --change-batch '{
    "Changes": [{
      "Action": "CREATE",
      "ResourceRecordSet": {
        "Name": "dns.urlstatuschecker.com",
        "Type": "A",
        "AliasTarget": {
          "HostedZoneId": "Z2FDTNDATAQYW2",
          "DNSName": "d3fu5iavm1zuit.cloudfront.net",
          "EvaluateTargetHealth": false
        }
      }
    }]
  }'
```

### Via AWS Console
1. Go to [Route 53 Console](https://console.aws.amazon.com/route53/)
2. Click **Hosted zones**
3. Click **urlstatuschecker.com**
4. Click **Create record**
5. Enter subdomain name (e.g., `dns`)
6. Select record type (A, CNAME, etc.)
7. Enter value
8. Click **Create records**

## 🚨 Troubleshooting

### Issue: Site not loading after nameserver change
**Solution**: Wait 24-48 hours for full propagation. Check with `dig` command.

### Issue: SSL certificate errors
**Solution**: Ensure CloudFront distribution has valid ACM certificate for domain.

### Issue: API not working
**Solution**: Verify `api.urlstatuschecker.com` CNAME record is correct.

### Issue: Email stops working
**Solution**: If you have email on IONOS, add MX records to Route 53 before migration:
```bash
aws route53 change-resource-record-sets \
  --hosted-zone-id Z0400631USJUHW2VUTMO \
  --change-batch file://mx-records.json
```

## 📊 Cost Estimate

### Route 53 Costs
- **Hosted Zone**: $0.50/month
- **Standard Queries**: $0.40 per million queries
- **Alias Queries** (to CloudFront): FREE

### Estimated Monthly Cost
- **Low Traffic** (10K queries/day): $0.50/month
- **Medium Traffic** (100K queries/day): $1.70/month
- **High Traffic** (1M queries/day): $12.50/month

## 📞 Support

If you encounter issues during migration:

### AWS Support
- [Route 53 Documentation](https://docs.aws.amazon.com/route53/)
- [AWS Support Center](https://console.aws.amazon.com/support/)

### IONOS Support
- [IONOS Help Center](https://www.ionos.com/help/)
- Phone: 1-866-991-2631

### Project Support
- Email: snapitsaas@gmail.com
- GitHub: [urlstatuschecker.com](https://github.com/terrellflautt/urlstatuschecker.com)

---

## ✨ Next Steps After Migration

1. ✅ Test site loading: https://urlstatuschecker.com
2. ✅ Test API: https://api.urlstatuschecker.com/health
3. ✅ Add DNS subdomain for propagation checker
4. ✅ Set up CloudWatch alarms for Route 53 health checks
5. ✅ Configure automatic failover (if needed)

**Migration prepared by Claude Code**
**Date**: October 6, 2025
