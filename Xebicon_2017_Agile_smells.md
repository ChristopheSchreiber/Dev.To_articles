On November, 30th 2017, I've attented to the XebiCon conference, a conference organized by Xebia society and its partners, in which many subjects were discussed, such as technical subjects (Kotlin, Big Data, IoT, Blockchain, DevOps...), organisational aspects (Feature teams...) and of course several talk about Agile methodologies.
One very interesting talk was the one from Julien Rossignol about Agile Smells, those signs that indicate something is wrong in your team organisation.
This talk was based on a series of articles he published on [Xebia's blog](https://blog.xebia.fr/2017/03/15/agile-agile-smells-management-visuel/) (in French) in the beginning of 2017.

## Lack of visual management
As a reminder, visual management consists in using the walls of your office to display information about your team's life: in progress tasks, its objectives, its potential issues... This allows to everyone passing by to quickly know what the team is currently working on.
For Julieu Rossignol, an Agile team's office with blank walls (no post-it, burndown chart or any other information) is a real sign of important problem, for instance:
* External pressure from the management, leading to the team hiding information to protect themselves
* A lack of product perspective and of medium to long term objectives, which can lead to loss of motivation for team members
* Usage of digital only tools, which can not be suitable for everyone and may lead to members being less involved during ceremonies
Here is an example of good visual management that Julien provided during the conference :
![Visual Management](https://www.christopheschreiber.fr/blog/wp-content/uploads/2017/11/management_visuel.jpg)
As a conclusion for this part, Julien expose the 3 characteristics of a good workspace:
* audibility: anyone should be able to easily hear and speak to the rest of the team
* visibility: every team members should see each other
* isolation: you should be able to discuss without disturbing the rest of the team

## Taskboard : user stories stay in the In Progress column
Julian described a simple example: since development tasks stay in the In Progress column after the development has ended, let's add a Validation column.
After that, we see that a Review column would be useful, then a Test one, then Deploy...
Multiplying the columns in the taskboard shows again a malfunction in the team:
* the need for multiple validation steps could indicate that the Product Owner is not present enough
* there is no testing best practices
* there is no team spirit, explaining why code reviews are so long to be done
* there is no OPS profile, which makes deployment slower

This also has consequences on the Sprint proceeding:
* there are a lot of ongoing user stories
* stories come back and forth between columns(for instance In Progress -> Validation -> In Progress -> Validation...)
* these are in fact disguised waiting lines
* collaboration between team members is not encouraged, since no one makes the effort to take stories out of In Progress column

Julien proposed several options to solve this problem:
* remove unnecessary columns. According to him, 4 columns are more than enough for most teams : Ready | Todo | In Progress | Done
* limit the work in progress (like in Kanban method) to favor team spirit. Developers will have to help each other in order to add new stories, using pair programming, code reviews...
* if possible, it is best to have teams where everyone can work on any topic, rather than having dedicated experts for each domain


## Being late for the Morning Meeting
This is a classic issue that every Scrum team must have faced :-P
The bad idea is to postpone the meeting to a later time so that everyone can attend it. After a few weeks, you'd probably need to postpone it even later because of new delays of the teams members. By doing this, you're only treating the symptoms, not the problem itself. Delays are often a sign of lack of interest for this ceremony.
Again, Julien proposed several ways of solving this issue:
* control the duration of the meeting by limiting each member's speaking time and postpone discussions after the meeting
* change the format : instead of letting everyone speak successively, you can try to focus on each story, and everyone involved in this story can talk about what is meaningful to say, avoiding repetitions between persons. You can also try a more informal meeting, in front of a coffee for instance, but from experience this is not a very good alternative according to Julien.
* change the organisation of the Sprint: try to have fewer objectives and stories that are coherent with them, limit the work in progress, encourage pair programming...
* if the team really think there is no point in this meeting, you can even cancel it definitely, for instance when team members already know what the rest of the team is working on and have good communication during the day (especially for small teams)


## Loss of attention during meeting
* According to some studies, the average attention time for an adult is 52 minutes. So, long meetings have negative effects on the participants, as they can lose their focus, take their phones... Julien advises to ask the participants to disconnect so that everyone can stay focused.
* As said previously, duration of the daily meeting should be limited
* Sprint Planning has to be prepared by working on the Backlog. You should avoid having too technical discussions and focus on the business aspects, with fast estimation decisions.
* Sprint Review must be prepared, practiced, so that it can be efficient, with as few slides as possible. Don't detail everything that has been done, tested or not...
* During retrospective, it is better if you focus on one or two problems instead of solving them all.
 
  
## Retrospectives where problemms are not raised or are too vague
Beware of the routine ! If few issues are raised, maybe it means that everything is alright, or maybe developers have forgotten the issues faced during the Sprint, or worse they think the retrospective is useless.

You should put the retrospective in context with what happened during the Sprint, by using adapted formats.
 * Assign a role to each member of the team
 * Define real actions (they should be [SMART](https://en.wikipedia.org/wiki/SMART_criteria))
 * Evaluate the retrospective to know if the meeting was useful 
 
## Backlog with too many details
If the backlog becomes very long, with a lot of details, we are close to a waterfall methodology, because we will tend to follow the initial plan, losing the advantages of Agile. Moreover, if items are discarded or replaced with other stories because of what has already been achieved or new constraints, every business or technical analysis previously done is lost, which can be frustrating for the team.étude et les analyses techniques effectuées, ce qui peut être frustrant.
 Julien's advice is to have a pyramidal backlog:
 * detailed stories for the next 2 or 3 iterations
 * less detailed features for the content of the current release
 * epics with few details for next release's features
 Backlog and user stories will be refined during iterations.
 
## Roles are not clear
The most frequent example is the role of the Scrum Master, which can be difficult to understand for the team. This is a sign of an issue of the team's structure and it can lead to conflicts and tensions.
 You should redefine the role to make it clear to everyone:
 * you can use the Stop / Start / Continue methode to know what is fine, what is wrong or what should be done
 * build your own Scrum Master : talk with the team to know what they think the role of the Scrum Master should be
 
Another role that can be unclear is the Technical Lead. If is omnipresent, it can lead to a decreased motivation of the developers because they will lack autonomy or learning...
 
## Difference between Agile Coach and Scrum Master
The Agile Coach:
 * observs the team
 * speaks only with the Scrum Master
 * may lead a community of Scrum Masters
 * lead the ceremonies alongside the Scrum Master
 
The risk for the Scrum Master is losing his legitimity. The can indicate that efforts are only directed on organisation and management. In such a case, we can wonder if being Agile is enough.
 Indeed, the technical part should not be forgotten:
 * you can add a Craft Coach to improve development practices
 * improving the design and the tests is also important
 * you also need to have better tools is delivery is an issue (DevOps)
 
## Conclusion
You shouldn't blindly follow a methodology and its ceremonies, you absolutely have to adapt it to your team and to the project's context.
You need to measure the efficiency of the process, for instance the time required for a feature to change column on the taskboard, and review during the retrospective what can be improved according to these measures.

