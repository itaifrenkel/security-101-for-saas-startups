Startups also can't efford engaging with multiple security vendors. This vendor narrowing tool should help you with preparing a shortlist by entering your specific startup requirements, and getting a list of vendors and product names. We also provide sample requirements to get you started.

## Disclaimers ##
* This tool should be used for narrowing down a selection of security vendors (vendor shortlist) and should not be solely used for making a purchase decision. The information is provided as-is and we cannot take responsibility for your security tools choices.  You would still need to contact the vendor, verify claims, read reviews and ask for a POC, before making a purchasing decision. 
* The information cataloged here is incomplete, inaccurate and out-of-date. If you find such an issue please open a Pull Request that includes the requested change and a link to a screencast showing that feature or technical documentation of the product. All vendors use massive marketing mambo-jambo, however they would not fake a screencast or their own official technical documentation. 
* The information cataloged here is licensed under a Creative Commons Attribution 4.0 International License. Feel free to fork this repo and change it.


## This tool is highly opinionated ##
* Security systems, just like your systems, requires integration, continous monitoring and upgrades. If you choose a tool that is too complex for your startup, you would be wasting time and money. This is why you should first read the "Security 101 for SaaS startups guide". 
* This is also why this catalog focuses solely on SaaS (hosted) solutions, since they were built from the ground up with usability in mind. The flip side of relying on a hosted security solutions is that if such a SaaS is compromized, many companies including yours might suffer collateral damage. Therefore, we focus on vendors that have soc-2 certification or are in the process of obtaining such certification.
* Even if you do find a security tool that is perfect for your organization, it would still be used by imperfect humans. Therfore we focus on tools that have identity baked into them. The minimum is users,roles/groups and devices. Hopefully it also provides role based policies (e.g. finance and management must have X feautre enabled on their laptop and mobile phones). At the end of the day, for most alerts you would contact the employee and walk them through whatever they did wrong (e.g. please don't browse website X using this laptop, ok?).
* The most important security tool is upgrading to the latest version of Windows or MacOS, and installing the latest security updates. If you're not doing even that, stop reading now, fix this, and come back when you're done. 
* Ubuntu Desktop is a great operating system too, but it's not covered here. The reason is that most security vendors make a living by selling to enterprise customers, and they rarly use Ubuntu Desktop, so the vendor selection is very limited.

## Prerequisites ##
```
git clone forter/security-101
docker run --name security-101 forter/security-101
```

## Basic Usage ##
We provide a sample of security requirements yaml files that include "must", "want", "nice", "exclude" requirement levels. They do not allow complex OR conditions, but should suffice for most usages. 
`docker exec -d security-101 vendors filter < requirements.yaml`

## Advanced Usage ##
You can use elasticsearch query syntax for more advanced queries
```
docker exec -d security-101 vendors querygen < requirements.yaml > query.json
<manually edit query.json>
docker exec -d security-101 vendors query < query.json
```

## Security Catalog criteria ##
| criteria | description |
|----------|-------------|
|vendor.name |  The name of the company producing the security tool/serivce |

vendor.headquarters # The country in which the company is listed. This is important when you want to exclude/include seucurity tools from countries that have specific regulations.
suite.name # The marketing name for the suite of tools that ussually have (or will have) a consolidated management console.

virustotal_malware - endpoint protection vendors registered with virustotal enjoy a stream of the latest malware.
virustotal_unsafe_websites - endpoint protection vendors registered with virustotal enjoy a stream of the latest urls
