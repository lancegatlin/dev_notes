- Best practice/standards discussions
	- Goal: discover how to code "less wrong" into the future 
		- Lesser goal: maybe rewriting legacy code
	- Goal: inspire engagement
	- Goal: invite critique
	- Goal: discover and learn what we don't know that we don't know
		- existing code
		- business requirements
	- Goal: help assess marginal value of coding better (important for prioritizing)
	- Goal: focus on influencing team's defacto standards
- Standards
	- Official team standards can be excellent
		- but agreement is difficult and time consuming
		- too many rules can become burdensome and brittle
	- Defacto standards that occur organically are better
		- though this can create anti-productive competing standards
		- can get stuck repeating code patterns that could be better
		- changing defacto code standards requires discussion, critique and persuading team to adopt new practices
	- Coding better is generally its own reward, therefore:
		- Prefer persuasion over dictates
	- But mixture of both offical and defacto standards is best
- My Tech Debt Philosophy
	- Generally not reasonable to rewrite legacy code when better ways to code are identified
		- Very common in Scala 
		- Scala ecosystem is constantly getting better
	- Some reasonable ways to deal with tech debt:
		1. Write specific tech debt stories as business allows
		2. Embed tech debt work in story estimates
		3. Rewrite legacy code as stories touch legacy code
		4. Write smaller microservices that can be eventually rewritten from scratch
		5. Slice off smaller microservices that can be rewritten from scatch