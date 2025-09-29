---
Tags: 
Created: 2025-09-23 10:37:12
---
(Links:: [[Network Security]])
# Example BGP errors
- There are many cases of BGP hijacking:
	- 2021 Facebook outage
		- Caused by misconfigurations, not an attack
		- may have opened a huge attack surface
	- 2024 Cloudflare DNS outage
		- multiple ISPs advertise 1.1.1.1 -> BGP hijack
		- Tier 1 provider accepted hijacked route as blockholing request

> [!question] Why are rerouting attacks so problematic?
> - **Metadata** can still be collected, regardless of encryption
> - **Traffic fingerprinting** is still possible, regardless of encryption or anonymity-enhancing techniques
> - Attracting traffic enables **attacks against protocols / end-hosts**

- Routing attacks can be used to...
	- **obtain fake TLS certificates**
	- **deanonymize TOR users**
	- **hijack DNS requests**
	- **partition the Bitcoin network**
# Mechanisms and Attacks
- originally built in 1989 -> no security mechanisms considered
- IP prefix *origination* into BGP
	- Prefix advertised/announced by the AS who owns the prefix • ... or by upstream provider(s) on its behalf
- IP prefix *hijacking*
	- A malicious (or misconfigured) AS announces a prefix it does not own

> [!error] Problem 1: BGP does not validate the origin of advertisements

## Prefix Hijacking

- ... 

> [!question] What can be done to the hijacked traffic?
> - Blackholed
> - Redirected
> - Intercepted (hard because we are advertising the IP ourselves)

## How to perform BGP *interception*
 - Advertise the prefix to a **select few** neighbours, although they may learn it from their neighbours
 - Using [[BGP poisoning]]
 - Using *BGP communities* to ensure the announcement only reaches certain ASes
	 - `NoExportSelect` action stops other ASes from forwarding announcement
> [!info] Hot to do BGP hijacking in 3 steps
> 1. Setup an AS and border router or compromise someone else's router
> 2. Configure router to originate the target (sub-)prefix
> 3. Get other ASes to accept the wrong route
> 	1. Many ASes do not discard wrong routes (no or insufficient filtering)

___
> [!error] Problem 2: BGP does not validate the content of advertisements

## ASes can modify the BGP path
<svg viewBox="0 0 310 100" width="100%" height="20vh" xmlns="http://www.w3.org/2000/svg">
	<path id="lineAC" d="M 50 50 q 105 -50 210 0" stroke="var(--color-red,red)" stroke-width="1,5" fill="none"/>
	
	<circle cx="50"  cy="50" r="5" fill="var(--text-normal,black)"></circle>
	<circle cx="155" cy="50" r="5" fill="var(--text-normal,black)"></circle>
	<circle cx="260" cy="50" r="5" fill="var(--color-red,red)"></circle>
	<line x1="55" y1="50" x2="150" y2="50" stroke-dasharray="5" stroke="var(--text-normal,black)"></line>
	<line x1="160" y1="50" x2="255" y2="50" stroke-dasharray="5" stroke="var(--text-normal,black)"></line>
	
	<text x="12" y="52">1)</text>
	<text x="32" y="70">AS 559</text>
	<text x="135" y="70">AS 3356</text>
	<text x="240" y="70">AS 8447</text>
	<text x="30" y="10" fill="var(--color-red,red) !important">BGP announcement direction: Removal</text>
</svg>
<svg viewBox="0 0 310 100" width="100%" height="20vh" xmlns="http://www.w3.org/2000/svg">
	<path id="lineAC" d="M 50 50 q 105 -50 210 0" stroke="var(--color-red,red)" stroke-width="1,5" fill="none"/>
	
	<circle cx="50" cy="50" r="5" fill="var(--text-normal,black)"></circle>
	<line x1="55" y1="50" x2="255" y2="50" stroke-dasharray="5" stroke="var(--text-normal,black)"></line>
	<circle cx="155" cy="25" r="5" fill="var(--color-red,red)"></circle>
	<circle cx="260" cy="50" r="5" fill="var(--color-red,red)"></circle>
	
	<text x="12" y="52">2)</text>
	<text x="32" y="70">AS 559</text>
	<text x="135" y="42">AS 3356</text>
	<text x="240" y="70">AS 8447</text>
	<text x="30" y="10" fill="var(--color-red,red) !important">BGP announcement direction: Addition</text>
</svg>

- ASes either *remove* or *add* as many ASes to the path
	- motivations removal
		- their path becomes more attractive for others to send data along
		- receive data from sources that try to avoid a specific AS
	- motivations addition
		- Due to loop detection, `AS 3356` can't send data to `AS 559`
		- BGP poisoning: `AS 3356` will forward traffic based on other announcements
		- `AS 8447` seems to have more connections
- Removal: Only `AS 559` can tell that the path that `AS 8447` announces is a lie
- Addition: `AS 3356` (if it sees the route) and `AS 559` (but may not care) can tell if the path is a lie

## Obtaining Fake Certificates with ACME

==TODO: Make the diagram==

## Other Attacks on BGP
- [[Denial-of-service attacks]]
	- Overloading the link between BGP routers
- Sending bogus TCP packest
	- FIn/RST to close the session
	- SYN floods to overload the router
- Eavesdrop on or tamper with messages by tapping the link

# Countermeasures
## Best Current Practices (BCP)
> [!question] What properties do we want?
> 1. Validation of ownership over an IP prefix by asn AS
> 2. Validation of ASes on BGP (routing) path announcement

-  Secure the BGP peering session between routers (authentication)
- Filtering routes by prefix and AS path
- Filters to block unexpected control traffic
## RPKI
- Resource Public Key Infrastructure aims to **validate** an ASes ownership over resources
- RPKI cryptographically asserts the *cryptographic keys of ASes* and the AS numbers and IP prefixes they own
- Roots of trust are the five regional Internet registries (RIRs)
- AN RIR signs a statement that a particular AS
	- has the private key to a specified public key
	- and owns certain *resources*: IP prefixes and AS number(s)
### Origin Authentication of BGP Messages
- RPKI uses **Route Origination Authorizations** (**ROA**s) to indicate which AS may announce an IP prefix
	- It can restrict the prefix to a maximum length, preventing *sub-prefix hijacking*
- ROAs are signed and distributed by global repositories
- ROAs work without change to BGP (*out-of-band* checking)

![[Distribution of ROAs.svg|700]]

> [!info] Distribution of ROAs
> 1. ASes and RIRs create ROAs to upload to repositories
> 2. ASes frequently fetch repositories to verify signatures and store them in local caches
> 3. BGP routers in AS retrieve ROAs (must be encrypted) from cache servers
> 4. BGP update messages are cross referenced with ROA list
### Origin Authentication Operation
1. BGP routers in other ASes receive the announcement from AS $M$ for prefix $v$
2. BGP routers in other ASes check against ROAs in RPKI for prefix $v$
3. BGP routers would drop the announcement since no valid ROA for AS $M$

<svg viewBox="0 0 310 100" width="100%" height="20vh" xmlns="http://www.w3.org/2000/svg">
	<path id="lineAC" d="M 50 50 q 105 -50 210 0" stroke="var(--color-red,red)" stroke-width="1,5" fill="none"/>
	
	<circle cx="50" cy="50" r="5" fill="var(--text-normal,black)"></circle>
	<line x1="55" y1="50" x2="150" y2="50" stroke-dasharray="5" stroke="var(--text-normal,black)"></line>
	<circle cx="155" cy="50" r="5" fill="var(--text-normal,black)"></circle>
	<line x1="160" y1="50" x2="255" y2="50" stroke-dasharray="5" stroke="var(--text-normal,black)"></line>
	<circle cx="260" cy="50" r="5" fill="var(--color-red,red)"></circle>
	
	<text x="32" y="70" font-size="10" font-family="Arial" fill="var(--text-normal,black)">AS M</text>
	<text x="135" y="70" font-size="10" font-family="Arial" fill="var(--text-normal,black)">AS 3356</text>
	<text x="240" y="70" font-size="10" font-family="Arial" fill="var(--color-red,red)">AS 8447</text>
	<text x="70" y="10" font-size="10" font-family="Arial" fill="var(--color-red,red)">BGP announcement direction: Removal</text>
</svg>

- BGP routers in other ASes receive the announcement from AS M for prefix v
## BGPsec
- Standardized in [RFC 8205](https://tools.ietf.org/html/rfc8205)
- Prevents crafting a valid origin by prepending ASes, and path poisoning
- **Idea**: ASes sign the BGP path announcement as they pass
- Each AS can check signatures of prior ASes using RPKI

> [!error] This does not prevent an ASes from removing entries!
> Solution: Include the *next* AS in the signature

- Now the existence of each inter-domain link is confirmed by *both* ASes
- Legacy BGP routes exist, which secure ASes must still accept -> attackers can use legacy BGP to announce bogus routes
	- ASes may still prioritize specific routes that do implement BGPsec, although it may lead to loss in performance

> [!error] Problems of BGPsec deployment
> Unless security is priority, deployment remains large, and effects of limited implementation are minor
> Deployment challenges:
> - different message format
> - complete, accurate registries
> - public-key infrastructure

- You can still employ other methods:
	- Remember which ASes originate which prefixes
	- Remember AS-level edges and paths
	- Prefer routes you used before and delay adoption of unfamiliar routes
# Summary
- BGP wasn't designed with security in mind
	- Entities that participate need to be trusted
	- Most security solutions are simple patches
	- Fundamental security problems don't go away -> Redesign inter-domain routing
- 


---
References: