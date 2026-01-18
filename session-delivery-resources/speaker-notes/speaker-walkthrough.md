# [Video Walkthrough](https://www.youtube.com/watch?v=3fFXFpAYC4M) Transcript

0:00
Hello everyone and welcome. My name is April Gittens and I'm a principal cloud advocate at Microsoft. Today I'm going
0:07
to take you on a journey of how a developer prototyped, tested, and deployed a multimodal AI agent inside
0:15
Visual Studio Code using extensions designed to streamline the creation of
0:20
AI solutions. It's Saturday morning. Bruno Xiao stands
0:26
in his apartment living room, coffee in one hand and his phone in the other, snapping a photo of the space he's about
0:33
to transform. He's finally ready to paint. He knows the vibe he's going for.
0:39
Warm, cozy, maybe something with a little depth. But now he's staring at a bigger question. Eggshell or matte,
0:47
satin or semig gloss? He wants the walls to feel elegant but still hide
0:52
imperfections. He's not a pro. And one wrong decision could mean repainting
0:57
everything next weekend. Zava, his local home DIY store, carries all the
1:03
essentials. But finding the right finish, that's where things get tricky. And that's exactly where our story and
1:10
our app begins. Now, meet Serena. Serena's a developer
1:17
at Zava, and her job, it's to help customers like Bruno get the right recommendations without needing to
1:24
search for hours or stand in line at the paint counter. But she's got a constraint. Limited time, a huge product
1:31
catalog, and a business that needs results fast.
1:37
So, she builds Kora, a multimodal AI agent created and scaled with Microsoft
1:43
Foundry. Kora isn't just chatty. Kora takes Bruno's input about his project
1:48
and searches through Zaba's catalog to find exactly what Bruno needs. Bruno can submit photos to Kora and Kora can
1:55
process those photos as context when generating its product recommendations.
2:02
And today I'm going to show you how Serena did it all within Visual Studio
2:08
Code. In this session, we're going to explore developer grade agent building
2:13
from first prototype all the way to production. You'll see how Serena rapidly tested and compared Foundry
2:20
models, how she streamlined prompt engineering in the agent developer workflow with the agent builder in the
2:26
AI toolkit. We'll also see how she evaluated agent responses using both manual testing and AI assisted
2:33
evaluators directly within the AI toolkit. And finally, we'll talk about what it takes to move from a working
2:40
prototype to something that's production ready. Serena didn't just stop after the agent was responding correctly. She
2:47
thought about how to monitor, scale, and ship responsibly. You'll see how Serena
2:52
bridged the gap to production using Microsoft Foundry resources. Serena's story isn't just about building. It's
2:58
about shipping smarter with the right tools at every step. And that's what today's session is all about.
3:07
But let's zoom out for a second because Serena's not alone. Developers everywhere are building AI powered apps
3:14
right now from side projects to full-on production systems. And what we're seeing is this. The tooling for
3:21
generative AI has exploded. There are more models, APIs, vector databases, and
3:27
orchestration frameworks than ever before, which is exciting, but also
3:32
overwhelming. Because here's the reality. Building an AI prototype is
3:37
easy, but getting that prototype into production, that's the hard part.
3:45
That's where the idea of Gen AI Ops comes in. Just like DevOps brought structure to traditional software
3:52
development, Genai Ops is about bringing workflows, tools, and repeatability to
3:57
how we build, test, and ship AI experiences. In this session, you'll see what GI ops
4:04
looks like in practice using tools from Microsoft Foundry and the AI toolkit in
4:09
VS Code. You'll follow Serena's journey not just to build an agent, but to ship
4:14
one that's useful, trusted, and scalable. So, let's dive in.
4:21
The first real decision Serena faced wasn't tools or deployment. It was picking the right model to power Kora.
4:31
And this is where the AI toolkit for Visual Studio Code comes in. The AI toolkit is a full development
4:37
environment right inside Visual Studio Code that gives you everything you need to explore models, build agents,
4:43
evaluate responses, and deploy to Azure. It's built for developers who want to go
4:49
beyond quick experiments and actually build, test, and ship AI powered apps.
4:54
And at the core of that experience is the model catalog, your starting point for choosing and experimenting with
5:00
models. You can use GitHub hosted models which are free to use, no API key needed, and
5:07
they're ideal for prototyping or just trying out agent logic. Although they are rate limited, you do have the option
5:13
to leverage GitHub's pay as you go models. There's also the Azure hosted
5:18
models and these are for enterprisegrade apps where performance and scale and compliance matter.
5:25
We then have thirdparty APIs like OpenAI, Mistral, Coher or anything you
5:30
can hit with a REST endpoint. And there's also local models including
5:35
those running through Foundry local oama or your own custom setup.
5:44
And if you aren't sure about which model to choose, then GitHub C-Pilot is here to help. GitHub Copilot Agent Mode is
5:51
like having an autonomous peer programmer right inside your editor. In fact, GitHub Copilot Agent Mode can help
5:57
with the complete agent building experience. The AI toolkit embeds agent development workflows directly into
6:04
Visual Studio Code and GitHub Copilot, enabling you to transform ideas into production ready agents within minutes.
6:12
Equipped with several AI toolkit tools, GitHub copilot agent mode can recommend models, build and orchestrate agents
6:18
using the Microsoft agent framework, trace agent behavior, and evaluate agent
6:23
responses for quality. As for Serena, her first step was to ask GitHub Copilot
6:29
for a model recommendation. And let's look at how she did that. So here within
6:35
Visual Studio Code, I'm going to open up GitHub Copilot chat. Once this is open, I can expand this so that way I can see
6:42
a bit more on the screen. Now, right now, what we're going to need is a recommendation of model or models to
6:49
help run the Kora agent. And so, I'm passing in a prompt that says, I'm creating an agent for a home improvement
6:56
company. The agent uses the company's product catalog via an MCP server to recommend the right products for
7:02
customers based on their DOI project. Which language model should I use?
7:08
Now that that's been sent, what's going to happen is that GitHub Copilot is
7:13
going to leverage one of the AI toolkit tools that it has access to. And in this case, it's using the get AI model
7:20
guidance tool. This particular tool is going to assist GitHub copilot with
7:26
understanding the best practices around model recommendation. And we do seem to
7:31
have an answer here. So if I scroll up, I can see that the overall choice in model is GPT4.1 mini. It takes things
7:40
into consideration such as the quality score. So that's taking things like benchmarks into account. Then there's
7:47
costs. And this is going to be very helpful because Zava would like to have
7:53
a better understanding of how much these models are going to cost when they're
7:58
being used in production or at least to have an estimate of what their total
8:03
cost will be as customers are using the Kora agent. We also have additional information here with respect to
8:09
latency. We really want to ensure that the core agent doesn't necessarily take
8:14
too long to provide a response to customers. And then we also have information here about the context
8:20
window. There's information here about coding that's not as necessary for this agent, but it does call out that it does
8:29
excel in instruction following. Just below that, GitHub copilot recommended a
8:34
budget friendly alternative which is going to be GPT4.1 Nano. And also it does note here that if
8:43
we just want to get started with trying something free, we could try the GPT4.1
8:48
mini model using one of the GitHub hosted models. And so from here, the
8:56
next thing that we would need to do is actually go test out this model and
9:01
let's assess how well it performs against our given scenario. So to do
9:07
that, we're going to open up the model catalog in the AI toolkit. Within the model catalog,
9:15
we're able to see a variety of models that are going to be available for use.
9:21
And as we scroll down, we can apply different filters such as who's the model hosted by the model publisher
9:29
features as well. So knowing that we want to use a model that supports image attachment, that would be a really good
9:35
feature to apply here. And we can also look at model types. And then also if we need fine-tuning support, I already know
9:43
that I want to give the GPT4.1 mini model a try. And so I can search for that one directly.
9:55
And here I see that we have a couple of options available. There's one from GitHub. There's one from Microsoft
10:01
Foundry as well. And then there's also one from Open AI. Since Zava would like
10:07
to develop agents for Microsoft Foundry, we're going to use a Microsoft Foundry
10:13
model. So from here, what we would do is deploy that model. I happen to already
10:18
have it deployed, so I won't redeploy it. In addition to that, there was another model that was mentioned by
10:25
GitHub Copilot, and that's going to be GPT4.1 Nano.
10:31
Let's see where that model may be.
10:38
We do have that one available as a GitHub hosted model. So, we'll go ahead and add that model as well. What we'll
10:45
want to do now is test out these models in the model playground. So, I'll open back up the AI toolkit here and I'll go
10:53
to the model playground. Within the model playground, this is where we can come and compare two models at a time.
11:01
If you want to just test out one model, you would stay right here within this area. But I want to compare. And so I
11:09
want to start off with the GPT4.1 mini model and that's going to be the
11:16
foundry model. And I can also select the GPT4.1 nano model that's going to be the
11:23
GitHub hosted model. What we'll want to do now is pass in a
11:29
prompt that's going to be suitable for our given scenario. And so I have a prompt which is describe what's in the
11:36
image including colors of the objects. I happen to have that same image of
11:41
Bruno's living room available. So we're going to pass that one in. These are the sort of conversations that we can expect
11:47
Zava customers to have with the Kora agent. I'll go ahead and submit that. Notice that we only need to submit the
11:54
prompt once and it'll populate on the other side for the other model that
11:59
we're comparing with. And we do have responses as well. I do believe 4.1 Mini
12:06
was just a hair bit faster. But what we would do now at this step if we were in Serena's shoes is just review the
12:13
output, determine which of the two we like most, and then that's what we would move forward with. So at this point,
12:20
Serena's chosen her model. She's tested it in the playground, compared outputs,
12:25
and deployed a GPT4.1 mini foundry model. And that sets the foundation for
12:30
everything that comes next. But models on their own aren't enough. To really help customers like Bruno, Serena needs
12:37
to define how her agent should behave. What should it say and what should it know? And that's where the agent builder
12:43
comes in. It's where Serena brings her agent to life. Let's walk through how she did it.
12:51
Here within the AI toolkit, I've opened up the agent builder. With this open, I
12:57
can now create the Kora agent. Starting off with giving the agent a name. We'll
13:02
name the agent Kora as mentioned. And then we want to make sure that we've chosen the proper model. And in our
13:08
case, we want to use GPT4.1 Mini from Microsoft Foundry. Now that the model
13:13
has been loaded, we can pass in a system prompt for the agent. If you are challenged with creating system prompts,
13:20
then no problem because we have a feature in the AI toolkit where we can
13:25
generate a system prompt for you. What you'll need to do is pass in a description of the task that your agent
13:31
will be completing and then we're leveraging AI to generate the system
13:36
prompt for you. I happen to actually already have the system prompt that Serena used when creating the Kora
13:44
agent. So, I'll put that in here. Now, it is quite lengthy, so I won't read it in its entirety, but to get a
13:50
gist of what's happening here. The agent is intelligent, friendly, and it's
13:56
helping customers from the Zava brand, a home improvement brand. And then we go
14:02
on to define the agents role. We also include information about the
14:07
personality as well. In the event I would like some assistance with
14:13
improving the system prompt, we have a feature available in here that can help you out as well. Notice where we once
14:19
had the generate button. It now says improve. If you pass in a system prompt and you would like improvement, you can
14:27
choose the improve button and then you can describe what it is you want to change. Once you describe what it is you
14:33
want to change, we will also use AI again to help make the improvements. And then from there you can decide what you
14:40
want to keep, what you want to get rid of, so on and so forth. So I'll close that up because we're going to use the
14:46
one that Serena created. So now let's give this agent a try as it is.
14:54
We're going to submit a prompt that customers like Bruno may
15:00
submit. We're going to use his living room image. And it says, "Here's a photo of my living room. I'm not sure whether
15:05
I should go with eggshell or semig gloss. Can you tell which would work better based on the lighting and layout?
15:11
We're going to go ahead and submit that here to the Kora agent. And let's just see how the core agent responds. So Kora
15:21
is able to process what's happening in the image that we've passed in. Kora insists that it has a soft natural light
15:28
coming through a window and it's cozy, understated decor style. And so given
15:33
the ample light and the elegant but relaxed vibe, eggshell is what Kora is
15:38
recommending. And so from here, Kora is asking follow-up questions to the
15:45
customer so that way it can better determine whether the eggshell paint is
15:50
going to be most appropriate. But although we have this recommendation from Kora at this step, what is missing
15:58
is the fact that Kora does not yet have access to Zava's product catalog. And
16:06
this is where MCP or model context protocol comes into play.
16:13
So what is model context protocol? Think of MCP as a secured structured way for
16:18
your agents to interact with external tools or services. Whether that's a database, a calendar, a weather API, or
16:26
in Serena's case, a product catalog. For Serena, this means that Kora can stay
16:31
focused on the conversation while the heavy lifting like looking up products is delegated to a tool behind the
16:37
scenes. And because it's all orchestrated through MCP, Kora doesn't need to know how the product catalog
16:44
works, Kora just needs to know that there's a get products by name tool that can be called. So let's take a look at
16:50
how Serena connected Kora to Zava's MCP server which is backed by a Postgress SQL product catalog.
16:58
So back here in the agent builder I can connect to Zava's custom MCP server and add whichever tools will be relevant for
17:04
Kora. Salva's basic customer sales server enables Kora to do product searches by name with fuzzy matching to
17:10
get store specific product availability through rowle security and get real time
17:16
inventory levels and stock information. I happen to already have the server running here in the background within VS
17:23
code and I can access it here via the agent builder. to connect this server
17:29
and provide its tools to Kora. What I will do is scroll down here to this tools section and I can add the tools
17:37
via the MCP server option and I can use tools that are already added in VS Code.
17:44
Of course, however, if you would like to use a different server, you could also browse servers that are available and
17:49
you can also manually add servers and you can also create your own servers with the AI toolkit. But I already have
17:56
one running. So I'm going to select one of the ones that's running in VS Code. I
18:02
only need in this case the get products by name. So I'll just select that one
18:07
and then I'll click okay. And now that tool has been added. We're going to use
18:13
that same prompt from earlier with Kora to see which product Kora can recommend.
18:20
Now that Kora has access to that database of products. going to start a new chat here and then I'll go ahead and
18:27
submit that prompt as well as the image of Bruno's living room and then we'll
18:33
see what the agent does in response.
18:39
Okay, we have another recommendation for eggshell paint and Kora does ask, would
18:45
I like the agent to recommend an eggshell paint finished product? I want to say yes. recommend an eggshell
18:54
paint from Zava and let's see if we get a tool call. If
19:00
we do get a tool call, it will display directly in the UI. And so I can see
19:06
that we have one tool call that occurred for get products by name. That is the
19:11
tool that was added. And we have the actual product recommendation for the interior eggshell paint from Zava. We
19:19
also have the price and it's readily available in stock. So now that Kora is
19:25
up and running and connected to Zava's product catalog using MCP, Serena has a
19:30
working prototype. But before she ships it, she needs to know, is Kora actually
19:36
doing what Kora is supposed to do? And are the responses clear? Are they trustworthy? And are they actually
19:42
helpful for Zava's customers? Essentially, Serena wants to know, can she trust this
19:49
Kora agent to interact with real customers like Bruno? And this is where
19:54
evaluation comes in. Evaluation is a critical part of Genai
20:02
Ops because in the real world, users aren't grading on a curve. If your agent gives bad recommendations, users lose
20:10
trust fast. Serena uses the AI toolkit to evaluate responses in two ways.
20:16
First, there's manually, and this is where she looks at sample prompts and responses to check for accuracy, tone,
20:22
and hallucinations. And then there's AI assistant, and that's where the AI toolkit runs
20:27
evaluators like task adherence and relevance across a test set of queries. And that scores the agents performance
20:33
at scale. These tools helps Arena catch issues early. She can also iterate faster and
20:40
build trust in the output all before going live. So, let me show you how Serena evaluates Kora inside the AI
20:47
toolkit. So, back here in the agent builder, we're going to switch over to the
20:53
evaluation tab. And this evaluation tab is what's going to enable us to start
20:59
running evaluations against the Kora agent. The first thing I want to call
21:04
out is our generate data feature. And that's going to be this starry icon here. In the event you want to evaluate
21:12
your agent, but you don't yet have data, we can generate that data for you. If you select it, you'll need to decide how
21:19
many rows of data you would like generated. And then we provide some generation logic based on the
21:24
instructions that have been provided for your agent. You're more than welcome to modify this just to ensure that you get
21:30
the proper set of data that you're looking for. I happen to actually already have the data that Serena used
21:38
when she was evaluating the Kora agent. So I'm going to enter that manually. I
21:46
can click add an empty row and I'll expand the screen so we can see it a little better. Here
21:52
I'll say what type of organic compost does Zava have?
21:59
And you could also import this data as well if you'd like. The next row of data
22:05
that I'll add in is does Zava have a paint bucket?
22:12
If so, how much is it? And let's do another one.
22:18
We'll add in what color glitter does Zava sell and then a fourth which will
22:27
be how much tape measure is currently in
22:33
stock. We have a little bit of variety here with respect to the sort of input that we could expect to receive from
22:40
Zava customers. So from here, what we'll need to do is run the agent so we can
22:46
get the agents response and then we can start doing our evaluations. You can run
22:51
all these at once or individually. I'm going to run them all at once. And so I'll select run response. And then
23:00
ideally what we should hopefully see is a couple of tool calls because the core agent does have access to the tools from
23:08
Zava's MCP server that consists of the Zava products.
23:18
All right. And so we have a tool call that's kicking things off here for the very first line. And then we have
23:25
another for the second. So this is pretty good. This is exactly what we
23:31
want to see happening. And then we have a final one. All right. So if we take a
23:36
look at what we have for the output here, I do see that we have product specific information. And as we go
23:43
through, so for the first one, organic compost 40 lb is the type of fertilizer
23:48
that Zava has in the garden and outdoor category. It's a great choice. Since at
23:54
this state I want to do manual evaluations which involves a human
23:59
assessing the output of the model. I'm going to say that this particular row of data is a thumb up. As for the second
24:07
row, I do see that there is a paint bucket grid that was recommended, but
24:14
it's a paint tray. It's not necessarily a paint bucket itself. And so I would
24:21
say, you know what, this one's going to be a thumb down. The third row was a bit of a trick question because I know Zaba
24:27
does not actually sell glitter, but we still got a tool call to occur and Kora
24:34
was able to inform the customer that Zaba does not have any glitter products,
24:40
but alternative options are available depending on their needs. So, I will say
24:47
yes, I do think that response deserves a thumb up. And then the final one with
24:52
respect to how much tape measure is currently in stock, I can see that we have a response of 7,162
25:00
units. And so, I'll say yes, that is a thumb up. We can also have AI assess the
25:08
agent output as well, and that will be an AI assisted evaluation. To set that
25:13
up, what we'll do is add the evaluation itself. But at this stage, let's say I
25:19
don't quite know which evaluators to use. And that's okay because we could
25:26
use GitHub Copilot to help recommend evaluators. And to do so, we're going to
25:32
open back up GitHub Copilot from earlier. and we're going to pass in a new prompt that says, "Which evaluators
25:39
do you recommend that I should use to do evaluations for my agent?"
25:44
And so what we're going to see here is that GitHub Copilot is going to invoke
25:51
another AI toolkit tool, which is the evaluation planner.
25:58
If I was working with the code files for this agent, then GitHub Copilot would
26:05
take what's in the project into account as it starts working through this workflow of creating the AI assisted
26:13
evaluations for my agent. But because we're still working in the toolkit, that's not available. But that's okay
26:20
because without that, we can still get evaluator recommendations. And there's
26:25
two here in particular that I do believe I want to focus on. One is the relevance evaluator as well as the coherence
26:32
evaluator. This feature is very helpful if you are challenged with creating
26:38
evaluations. GitHub copilot provides some very helpful guidance and will take you
26:44
through the full workflow of choosing your evaluators, creating data sets if
26:49
you don't have one, and then also creating the evaluation script as well. Our evaluations are using the Azure AI
26:56
evaluation SDK and its built-in evaluators. But alternatively, you could also create custom evaluators as well.
27:05
For now, I'm going to use the built-in evaluators that we have. So, heading back into add evaluation, I'm going to
27:11
use relevance and coherence and click okay. And now I need to choose the
27:17
language model that I want to use as the AI that does these AI assisted
27:22
evaluations. I want to use GPT4.1 Mini. And then from there,
27:29
what we're going to have are the relevance and coherence columns added to
27:36
our evaluations for our agent. You can run all evaluations at once or
27:42
you can do them one at a time. I'm going to run the evaluations for the very
27:47
first row. So I'll just select that one followed by run evaluation only. And
27:54
what this is going to do is assess the output about the organic compost
28:00
fertilizer for its relevance and coherence.
28:08
Okay. So we do have output here and you can either view the output in this view.
28:14
Alternatively, you can actually open up this particular row of data and this
28:20
will bring you into viewing information by the row in particular. Not only do
28:25
you have the input itself, but we have the evaluation results as well. And I can see that out of a range of one to
28:33
five with five being the highest relevance scored a four. and then
28:38
coherence score to four as well. When you complete evaluations with the Azure
28:44
AI evaluation SDK, you do also receive the relevance reason, you get the
28:49
coherence reason, and you'll get the reasoning for the other built-in evaluators as well. In addition to that,
28:55
over on the right hand side, I can also see the model response, any tool response, and then the final model
29:02
response, which is going to be the output that goes directly to our end user.
29:09
Evaluating agents is only effective if your comparisons are well structured.
29:15
Random sidebysides won't reveal what's actually better. You need a consistent,
29:20
thoughtful approach to how you test. Otherwise, you risk misinterpreting the results or chasing improvements that
29:26
aren't really there. But here's how to make your evaluations count. Change one variable at a time. If version A uses
29:34
GPT-4 and version B uses GPT40 and the new tool and the new prompt, you won't
29:40
know which change made the difference. Isolate those variables so you can learn what's actually happening.
29:48
Also, use the same test prompts. Run each version of your agent against the same scenarios, ideally ones that
29:54
reflect real user needs or known edge cases. You want to see how each version
30:00
handles the exact same input. Also, evaluate against the same
30:06
criteria. So whether you're using manual scoring such as, was this fluent, helpful, or grounded, or automated
30:12
metrics, keep your rubric consistent so you can compare apples to apples. Also,
30:17
keep notes or a change log. Even a simple note like V3 ads retrieval tool and use a shorter prompt helps you track
30:24
what changed and makes it easier to revisit decisions later if needed. Also
30:30
watch for trade-offs. Sometimes one version improves relevance but loses fluency or speeds up performance but
30:36
drops grounding. Testing helps you surface these tensions so you can prioritize intentionally.
30:43
The goal isn't just to find the best version. It's to understand why one works better than another.
30:49
And that insight is what makes your agent stronger with every iteration. All right, so back to Serena and the Kora
30:56
agent. At this stage, Serena has validated that Kora is behaving as expected. She's tested the responses,
31:03
tuned the prompts, and validated the output. But a working agent isn't the
31:08
same as a working app. Next, Serena needed to integrate the agent logic into
31:14
a realworld product and get it deployed. For now, let's focus on getting the
31:21
agent into a realworld product. One of Serena's colleagues created the UI for the Kora app and pass along the project
31:28
files. Therefore, the front end is complete, but what's missing is the brain behind it, the agent logic, the
31:35
call to the MCP server, and the model responses. Within the AI toolkit, Serena
31:41
can export the agent code via her preferred SDK. And so as we scroll down
31:46
at the bottom, we're going to have an option to view the code. And then from here, we have three options available.
31:53
Either the Azure AI inference SDK, semantic kernel SDK, and then we also
31:58
have the Microsoft agent framework. Serena chose to go with the Microsoft agent framework in which Python was her
32:04
chosen language. And now we have the code file for the agent that was created
32:10
within the agent builder. But after getting the code for the agent, Serena still needs to merge her agent code with
32:17
the existing app code. So rather than spend cycles going through her colleagueu's project, she could instead
32:24
leverage GitHub copilot again to merge the agent logic with the front-end logic
32:33
as well and have a fully functional app. And I actually happen to have that app
32:40
here all put together. So remember Bruno, let's pretend we're Bruno for a
32:47
second. And what we're going to need is a recommendation for painting the living room. I'll pass in that prompt from
32:54
earlier which states, "Here's a picture of my living room. I'm not sure whether I should go with eggshell or semig
32:59
gloss. Can you tell me which would work better based on the lighting and layout?" We're going to pass in that
33:05
image of Bruno's living room and let's see how the agent does. So Kora is
33:12
working on it and then soon here we should get a response from Kora based on
33:17
what's been processed in that image. So Kora has taken the image into account
33:24
based on the soft natural light with the clean and cozy aesthetic. We have the
33:31
things to consider if you want to go the eggshell route or the semig gloss route. And in this case, Kora is recommending
33:38
the eggshell paint. And we do want recommendations for the Zava eggshell paint. So, let's
33:45
ask Kora recommend a Zava eggshell
33:50
paint. Cora is working on it again. Somewhere behind the scenes, Cora is doing a tool
33:57
call. And in doing that tool call, Kora is able to find the interior eggshell
34:03
paint from Zava and Kora follows up to ask would we like more details on his
34:08
product or assistance with purchasing. Naturally, I want to know how much is
34:13
the product. And in asking that question, Kora is
34:20
able to inform me that the eggshell paint from Zava is priced at $6567.
34:27
What you've just seen here is that the Kora agent has done two tool calls
34:33
successfully. And very quickly, we have gone from prototyping this core agent and adding
34:40
that logic into an existing app. So that way Zava can place their core agent into
34:49
production for their customers. With a working app in hand, the final
34:55
step is getting it out into the world. Serena used GitHub Copilot for Azure to
35:02
guide her through the deployment process. It's not just generic advice. GitHub Copilot understands a project
35:09
context and provides stepbystep instructions to prep it for Azure. In
35:15
Serena's case, that meant configuring her app to run as an Azure container
35:20
app. What's powerful here is that she doesn't need deep Azure expertise, just
35:26
a clear goal and the right tools. GitHub C-Pilot for Azure lowers the barrier to
35:32
entry so developers like Serena can ship with confidence even if they're new to cloud deployment. And for Zava, this is
35:39
where the investment pays off. They now have a real scalable agent experience
35:44
running securely on Azure with the ability to manage, monitor, and evolve it over time. But this stage isn't just
35:52
about pushing code. It's where Zava starts to realize the full value of the Azure ecosystem.
35:59
By deploying to Azure, Zava gains more than just compute. They get enterprisegrade infrastructure,
36:06
security, governance, and life cycle management tailored for generative AI workloads. Let's unpack what that
36:13
unlocks for Zava operationally and strategically. First, Zava gains
36:18
built-in security and compliance. There's role-based access controls, managed identities and secret handling
36:25
and policy enforcement and enterprisegrade governance. Second, Sappa gains access to a centralized
36:32
model and agent management system. There's version deployments, agent registries, and built-in evaluation
36:38
pipelines. And finally, Zava benefits from scalable
36:43
infrastructure that's purpose-built for generative AI. There's elastic model serving which is the ability to
36:49
automatically scale AI model inference infrastructure up or down based on demand. So your application remains
36:56
responsive without overprovisioning resources and also there's visibility into cost and usage metrics. This is
37:03
what Microsoft foundry unlocks for Zava a production ready secure platform that supports not just the launch but the
37:10
life cycle of their AI powered customer experience. And of course, once Kora is
37:16
live, Zava needs to keep everything healthy, trustworthy, and measurable. And that's where observability comes in.
37:26
Once Kora is live, Serena and the Zava team need more than just logs. They need real visibility into how the system is
37:33
performing. That's where observability becomes critical. For example, let's say
37:38
Bruno, our painting customer, messages Kora and gets no product recommendation
37:44
in return. Is it a model issue, a back-end error? Did the MCP tool call
37:49
fail? With Foundry control plane, Serena can trace the entire flow end to end
37:54
from the initial user message to the agents tool call and to the agents response. The Microsoft Foundry control
38:01
plane is a unified management interface that provides visibility, governance, and control for AI agents, models, and
38:08
tools across your Foundry Enterprise. Serena can drill into latency across
38:13
steps, failures, and tool calls or model responses, and even see custom metrics that she's defined, such as evaluators
38:20
to assess the agents friendliness. This level of observability means Zava can detect problems faster, resolve issues
38:28
proactively, and deliver a more reliable customer experience.
38:36
Today, we walk through what it really looks like to go from idea to impact.
38:41
From building your first agent to deploying a productionready AI agent solution backed by Microsoft Foundry,
38:48
you saw how tools like the AI toolkit, GitHub copilot, and resources within Microsoft Foundry fit together in a
38:56
developer first gen AAI workflow. And you saw how Serena working under
39:01
pressure with limited bandwidth was able to still ship something powerful,
39:06
scalable, and responsible.
39:12
Whether you're a solo developer, part of a startup, or scaling enterprise systems, these tools are available to
39:19
you right now. You can explore the AI toolkits agent builder inside Visual Studio Code and deploy confidently with
39:26
Microsoft Foundry. The future of AI development isn't about code or creativity alone. It's about how fast
39:33
you can turn an idea into something real. And now you've got the tools to do it.
39:41
Thank you. You can download today's presentation by scanning a QR code or you can visit
39:49
aka.ms/microsoft/brk441.
39:57
And if you're looking for the next steps to advance your AI expertise, check out our apps data dev skills. And you can
40:04
also scan again for access to the repo for this session.