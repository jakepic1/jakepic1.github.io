# Designing a system for designing systems

_April 2022_

“Software architecture” is an odd term, because “architecture” implies that you’re building something inorganic, like a skyscraper. But as Fred Brooks [observed](https://en.wikipedia.org/wiki/No_Silver_Bullet) in 1986, the best software systems aren’t _built_ so much as they’re _grown_. He recommended rapid prototyping to refine the requirements, then a certain amount of design to get the boundaries right, and filling in the implementation using incremental development. His argument was that software is too complex to specify every detail from the start.

This was true in Brooks’ day, and it’s even more true now. In a startup, or even a new project at a big company, you never have all the information. You may have an idea of what the product needs to do initially, and an even more vague idea of what it needs to do in the future. But without knowing every future product requirement, it’s impossible to avoid some amount of incremental development, so you might as well embrace it. And even if you know _exactly_ what the software needs to do, it’s incredibly difficult to architect any non-trivial system ahead of time.

There’s only one situation where you know precisely how to build a complex system: when it’s something you’ve done before. But most programmers hate duplicating work, so when you find yourself coding the same system multiple times, you usually create a reusable abstraction. As a result, most systems end up being partially novel – at least to the people developing them.

## Process
Because it’s impossible to design the perfect system up-front, we use a process: Agile. Many companies have bespoke “Agile” implementations that are comically terrible, so it understandably has a bad reputation with a lot of engineers. But the core [principles](http://agilemanifesto.org/principles.html) of Agile make sense: rapid iteration, self-organizing teams, working software at all times. In the best case, Agile _is_ a way of “growing” software.

The problem with Agile is that it’s very small-team-oriented: while it can work great within a 5 or 10 person team, the typical approaches don’t scale well when too many people get involved. Traditional management practices divide larger teams into smaller ones. But what happens when you have 3 teams that need to coordinate together? Suddenly, you have an organizational problem.

Companies try to solve this problem with another layer of Agile, at the cross-team level. Team leaders (and sometimes ICs) often come together in a weekly or bi-weekly standup. The time chunks are bigger, and so are the pieces of work you’re taking on. You still want each team to operate mostly independently, but you have to do more up-front design to make sure their components will fit together.[^agile] Each team gets a little piece that they can iterate on, and those pieces will hopefully combine into a beautiful gestalt.

[^agile]: There’s a spectrum here: as more teams get involved, Agile starts to look more like waterfall.

## Conway
Here we arrive at [Conway’s Law](https://en.wikipedia.org/wiki/Conway's_law): the system design will at best match the communication structures (i.e. org chart) of a company. If you have 3 separate teams, the best you can do is a system that has 3 separate services or components. It could be even worse – like if there are legacy systems from past reorgs, or if the software engineers aren’t very good and create needless complexity.

Conway’s Law has an important implication: system design is a people problem, not just a technology problem. If the best your software can do is match the org chart, then what really matters is the org chart. Directors and VPs are the real software architects.

This can be challenging in practice. Most managers that I’ve worked with complain about the amount of time they spend in meetings. Indeed, looking at their calendars, many of them are in meetings at least 8 hours a day. But system design (and the processes to support it) requires deep thought. Managers and directors should ideally attend fewer meetings, and devote more time to thinking hard about how to improve the communication structures and processes on their teams.[^thought]

[^thought]:  Carving out this time is difficult, but you could try is scheduling a recurring meeting with yourself to do deep work.

Engineering leadership should also have some understanding of system design. They don’t need to be the best programmers or know the latest hot technologies. Instead, it’s more important that they have a lot of experience working on larger projects – or they should work very closely with people who do. At minimum, managers should be aware that org changes directly impact the software, and consult with their best engineers before messing with any teams or processes.

As a byproduct of Conway’s Law, the product’s user interface will _also_ map to the organizational structure. This means engineering leaders depend on the product team, just as the individual engineers within a team have to work closely with their PM. This is unavoidable: if you’re making software to fulfill a product need, you’re going to have to model the software-making system after the needs of the product too.

## Organizing
This is all so complicated, and it’s complicated because _people_ are complicated: distributed systems are even more complex when they’re made of humans. People have to communicate to work together. The more people and teams there are, the more communication is required, and the more complex the resulting software will be.

It’s no wonder software development at big companies tends to slow to a crawl. There seems to be a tension here. Software products inherently have massive economies of scale: if you can make a product that millions of people want, the marginal cost to get it to them is incredibly cheap. But software _development_, the process of making software, often has diseconomies of scale. The larger the company gets, the more slowly it writes code.

You can reverse some of these diseconomies of scale through code reuse. This is why many companies have “platform” teams who make internal-facing libraries or services for the “product” teams to consume. But this only helps when the product teams have to do a lot of otherwise repetitive work – like when a game studio has a dedicated engine team. Game engines are a lot of work, and most game companies ship more than one game, so there’s a lot of reusable code (which is why smaller studios usually license an engine). But a lot of companies, especially startups, generally don’t have as many of these natural reusable pieces, aside from things like service monitoring. And as with smaller-scale forms of code reuse, it’s a bad idea to create platform teams before you know the reuse is really necessary. Specific code can be much simpler than generic code.

What else can managers do? I think that rather than ignoring or fighting Conway’s Law, we should embrace it. If you know that in the best case your product and software architecture will match the org chart, you can lean hard into that assumption. There are some principles you can follow when growing an organization.

_Minimize the number of teams and people._ A single team of 6 is often better than 3 teams of 3 people each (yes, even though it’s fewer people). Don’t “spin up a new team” until you have a real need – this is [YAGNI](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it) at the org level. Eventually you’ll need to split into sub-teams – [8 or more people by some metrics](https://lethain.com/sizing-engineering-teams/) – but don’t do this prematurely. It’ll take a while to staff up the team through hiring, anyway.

_Strive for high [cohesion](https://en.wikipedia.org/wiki/Cohesion_(computer_science)) within a team._ People should be on a team because they work together a lot. This can include cross-functional teams when appropriate.

_Teams should be [loosely coupled](https://en.wikipedia.org/wiki/Loose_coupling) to other teams._ This means fewer meetings for everyone. Actually, you can use meetings and emails as a cost function to minimize when optimizing the org chart: the less cross-team communication required to make any decision, the better.[^org_chart]

[^org_chart]:  You could visualize how bad your current org chart is by analyzing chat/email/meetings to see who communicates with whom, and looking for all cross-team chatter. If people are mostly working with sibling teams rather than cousins, you’re doing all right.

_Design the organization around the product needs._ Let the desired user experience drive the org chart, not the other way around.

_Minimize reorgs._ It’s not just that moving people around is disruptive for a while as the teams settle, it’s that software retains baggage from past reorgs. Shuffling teams is a proven way to create crushing tech debt.

## Product
There are some obvious challenges with the above recommendations, especially the last couple. If you’re unsure of exactly what product you’re building, how can you know what the UX should be? And if you don’t know that, how can you possibly avoid reorgs, especially as the product (or company) grows?[^grows]

[^grows]:  There’s an interesting parallel between what Brooks says and Conway’s Law: just like the software system, a company is grown more than it’s built.

This is why companies should use a true [MVP](https://en.wikipedia.org/wiki/Minimum_viable_product) or rapid prototyping model for new product development. It’s cliché, but a new product should look like a mini-startup within the larger company. It’s a new team, with prototype code, so don’t try to shoehorn it into the existing systems until it’s proven. You can rewrite it using the real code _after_ the the product-market fit is de-risked.

That’s right, I’m saying you should plan to [throw away](https://en.wikipedia.org/wiki/The_Mythical_Man-Month#The_pilot_system) the MVP. The main goal is just to test product and market risks. If possible, let the designers use a no-code tool to build the prototype. Get programmers involved if there are also implementation risks, or when the no-code solutions are insufficient. But make it a new system, tied as little as possible to the existing system. As [easy](https://www.youtube.com/watch?v=SxdOUGdseq4) as it is to reuse your existing services or databases, the resulting complexity will cause long-term grief. Gain speed from rapid prototyping tools, not from trying to reuse existing production code.

What if you need to integrate with an existing system in order to prove the product risk – say, the new product only makes sense if users can access their real data from the old product? In that case, keep the coupling as loose as you can. Use batch jobs, or event streams, or whatever else you need to do to keep the old code as unaware of the new code as possible. It’s fine to add general capabilities to an existing service, like exporting more data, but it should rarely be specific to the janky prototype of a new product.

## Architects
The above advice doesn't just apply to new products: you should always use the most loosely-coupled APIs possible (although what this means in practice depends on the needs of the API).[^coupling] Loose coupling poses more up-front design challenges, but it’s the most sane way to communicate between services if you don’t want the system-level equivalent of spaghetti code. As with all advice, there are exceptions, and you have to do what’s right for your company.

[^coupling]:  Note that even time can be loosely coupled. Say it with me: “Async is best; events, not REST!”

Making cross-team technical decisions like this is an important job of a software architect or CTO. This includes the API structure – e.g. use Kafka for events. Depending on the company, this may also be more implementation-specific, like only allowing certain programming languages or databases. Ideally, you wouldn’t need to do that, and every team could be truly self-organizing. But we’re living in a world where reorgs are inevitable, despite our best efforts. You don’t want the only Clojure service in the company to get hot-potatoed between all the Go programmers.

It’s not necessary to have people with an explicit “architect” title, of course. Any sufficiently senior engineer can make these decisions. But whatever the titles, _someone_ should do this work. It’s better to consciously choose standards than to accidentally wind up with them through tribal knowledge.

Architects or high-level engineers have other responsibilities beyond choosing technical standards. They should also provide input to management before any reorg, to ensure the resulting teams can produce a quality system with minimal coordination. They can hopefully still code and design. And they’ll spend a lot of their time doing more “soft skill” work, like leading design reviews and mediating major cross-team efforts.[^soft_skills]

[^soft_skills]:  I think it’s actually more important that high-level engineers have people skills than that managers have technical skills. So if someone is both a good engineer and a good communicator, they shouldn’t always go into management.

## Design reviews
On that note: sometimes, you’ll need to create a new API, or add to an existing API, or some other technical change that affects people outside your immediate team. Some communication between teams is unavoidable at this point, so it’s a good idea to write a design doc. If you’ve structured things correctly, the main thing other teams need to know is just the API you intend to provide. Some implementation details _might_ be salient, like if you want to use a risky new technology, but including these in cross-team communication is often a warning sign that teams are too tightly coupled.

You might do some up-front implementation design within your team as well, like if you need to change a database schema. In this case, you may want two design docs: a very brief one for external consumption, and the more detailed one for your team. Note that depending on the team and project, whiteboarding sessions are often better than formal design docs for the implementation – but it’s still usually a good idea to write things down. In case it’s not obvious, you should design the API before spending too much time on the implementation details (unless the details are relevant to the API).

After you write the API design, you need to share it with other teams. Informal, one-off design reviews are fine if done rarely. But if they start happening frequently, you may need a review process.

What should such a process look like? Again, we’re trying to minimize communication overhead across teams, but no solution is perfect. Regular (e.g. weekly) meetings are high bandwidth and less interrupt-driven, but they can become a huge time sink.[^sink] An asynchronous method like email gives individuals more control over their time (if they have the discipline), but can easily pile up in inboxes and lead to heavy context switching.

[^sink]:  I often think of “sync” meetings as “sink” meetings.

Whatever review process you choose, remember that it’s _not_ about micromanaging designs or [quibbling](https://en.wikipedia.org/wiki/Law_of_triviality) over minor details. Build capable teams, then give them autonomy over the implementation. This helps to keep design docs themselves lightweight – just a page or two of high-level changes – which in turn makes it much easier for reviewers to find the time. And if the architects and managers have done their job, the less hands-on approach will lead to happier teams and reasonable software quality.[^micromanagement]

[^micromanagement]:  In general, micromanagement is not just bad management, it’s a sign of past poor management decisions. It’s like having to use willpower to stick to your diet at home because you previously bought candy at the grocery store.

## Teams
In the previous paragraph, I said “build capable teams,” but what does that mean? The recommendations are largely the same as for the whole company, but on a smaller scale. Use rapid prototyping wherever possible. Minimize communication overhead between individuals on the team: while colocation is best for increasing bandwidth, you can also reduce the amount of necessary communication by carefully partitioning the work. Give everyone something to “own,” even if this gets shaken up from time to time. Remember that the system is grown, not designed – and the team leader needs to foster that growth.

I don’t have any specific capital-P “Process” recommendations for teams, because I don’t believe that any one process or methodology is right for every team or company (particularly in the age of remote work). Besides, the art of growing an organization is to set up the teams to succeed _without_ becoming too top-heavy, and one the often-forgotten principles of Agile is that individuals should be emphasized over processes. To do this well, you also need to hire and retain good people – but that’s a topic for another day. I will just say that people are the most important ingredient in a team, and no amount of process can get around that (although a bad process can certainly _hurt_).[^process]

[^process]:  Every company claims to only hire the best, but then usually ends up imposing heavy processes as the company grows. This is almost an admission of not having the best people: the better the employees, the less process is necessary. It also becomes a self-fulfilling prophecy, because the best people don’t want to work at companies that have arduous processes.

## Bullets
Software development is always hard, and it gets harder as more people get involved. This seems to apply at every level, whether in a team, company, or industry. Perhaps someday we'll be free from this complexity. But Brooks wrote in the eighties that we are unlikely to see a productivity silver bullet within a decade, and that might still apply to the next decade. Instead, we need a lot of regular bullets, and that takes hard work.

Engineering leadership’s job is to grow a system (organization) that grows another system (software), remembering that the latter will reflect the former. In other words, good companies tend to produce good software – and don’t we all want to make both employees and customers happy? Don’t lose sight of that as you race to your next meeting.
