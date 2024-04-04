---
Tags: 
Created: 2024-03-18 16:01:29
---
(Links:: [[Human Computer Interaction]])
# Metaphors
> Usability is a quality attribute that assesses how easy user interfaces are to use. The word 'usability' also refers to methods for improving ease-of-use during the design process
> - Jakob Nielson

## 5 Quality Components
1. Learnability: How fast can a user learn how to use a system
2. Efficiency: Once users have learned the design, how quickly can they perform tasks?
3. Memorability: When users return to the design after a period of not using it, how easily can they. reestablish proficiency?
4. Errors: How many errors do users make, how severe are these errors, and how easily can they recover from the errors?
5. Satisfaction: How pleasant is it to use the design?
## Types of Design Rules
- Principles
	- abstract design rules
- Golden rules and heuristics
	- more concrete than principles
- Design patterns
	- generic solution for a specific problem
- Styles guides
	- provided for devices, operating systems, widget libraries
- Standards
	- (very) detailed design rules

Authority: Whether or not a rule must be followed or whether it is just suggested
Generality: Applied to many design situations or focused on specific application situation
# Principles to Support Usability by Dix
## Learnability
> The ease with which new users can begin effective interaction and achieve maximal performance

- Predictability
	- determining effect of future actions based on past interaction history
	- operation visibility
- Synthesizability
	- ability of the user to assess the effect of past operations on the current state
	- the user should see the changes of an operation
	- immediate vs eventual feedback
- Familiarity
	- how prior knowledge applies to a new system
	- affordance (guess-ability)
- Generalizability
	- extending specific interaction knowledge to new situations
- Consistency
	- likeness in input/output behavior arising from similar situations or task objectives
## Flexibility
> The multiplicity of ways the user and system exchange information

- Ways in which the user and the system exchange information
- Dialogue initiative
	- freedom from system imposed constraints on input dialogue
	- user preemptiveness: user initiates dialog
	- system preemptiveness: system initiates dialog

**Multithreading**
- ability of system to support user interaction for several tasks at a time
- concurrent multimodality: simultaneous communication of information pertaining to separate tasks
	- multi-model dialog
	- editing text and beep (incoming mail) at the same time
- interleaving multimodality: permits temporal overlap between separate tasks, dialog is restricted to a single task
	- window system, window=task
	- modal dialogs
	- interaction with just one window at a given time

**Task migratability**
- passing responsibility for task execution between user and system
- example: spell checking

**Substitutivity**
- allowing equivalent values of input and output to be substituted for each other
- representation multiplicity
- equal opportunity: blurs the distinction between input and output

**Customizability**
- modifiability of the user interface by the user (adaptability) or system (adaptivity)
- adaptability: users ability to adjust the form of input and output
- adaptivity: automatic customization of the user interface by the system
## Robustness
> The level of support provided to the user in determining successful achievement and assessment of goal-directed behavior


**Observability**
- ability of the user to evaluate the internal state of the system from its perceivable representation

**Recoverability**
- ability of the user to correct a recognized error
- reachability (states): forward (redo) / backward (undo) recovery
- commensurate effort (more effort / steps for deleting a file than for moving it)

**Task conformance**
- degree to which system services support all the user's tasks
- task completeness; task adequacy 

**Responsiveness**
- how the user perceives the rate of communication with the system
- preferred: short durations and instantaneous responses
- stability and indication of response time
# Principles for UI-Design
## Recognize User Diversity
- Simple and obvious - nevertheless in reality extremely difficult

> [!example] Online travel agent
> - Travel agent booking many flights a day - everyday
> - A teacher organizing a field trip (once a year) and making bookings for a large group
> - A business person changing bookings while travelling
> - A family looking for a package holiday

- Basic concepts to structure the problem
	- Usage profiles
	- Task profiles
- There is no average user! -> user persona (typical user)

**"Know Thy User"**
- What is the background of the user?
- Different people have different requirements for their interaction
- Issues to take into account:
	- goals, motivations, personality
	- education, cultural background, training
	- age, gender, physical abilities
- Experience:
	- Novice users
	- Knowledgeable intermittent users
	- Expert frequent users

> [!tip] Find out what the user is trying to do! The Goal!
> Needs of users, goals and resulting tasks

- Supported tasks should be determined before the design starts
- Functionality should only be added if identified to help solving tasks
	- Temptation: If additional functionality is cheap to include it is often done - this can seriously compromise the user interface concept (and potentially the whole software system)
- Frequency of tasks related to user profiles
## Follow the Eight Golden Rules
- Strive for consistency
- Enable frequent users to use shortcuts
- Offer informative feedback
- Design dialogues to yield closure
- Error prevention/handling
- Permit easy reversal of actions
- Support internal locus of control
- Reduce short-term memory load
### Strive for consistency
- Within an application it is the developer's job
- In a specific environment it is defined by guidelines (e.g. for GNOME, for KDE, for Mac OS, for Windows, for JAVA Swing)
- Consistency with regard to
	- Layout 
	- Function
	- Semantic
- In the WWW it gets pretty hard!
	- No real guidelines and no authority
	- How are links represented?
	- Where is the navigation?
	- Styles and "fashion" change quickly...
	
#### Consistency level
**Lexical**
- Coding consistent with common usage, e.g.
	- red = bad, green = good
	- left = less, right = more
- Consistent abbreviation rules
	- Equal length or first set of unambiguous chars
- Devices used same way in all phrases
	- Character delete key is always the same
	
**Syntactic** ^e295fc
- Error messages placed at same (logical) place
- Always give command first - or last
- Apply selection consistently, e.g. select text then apply tool or select tool and then apply to a text

**Semantic** ^c37730
- Global commands that are always available 
	- Help
	- Abort
	- Undo
- Operations valid on all reasonable objects
	- if object of class "X" can be deleted, so can object of class "Y"
#### Capture through Grammars
**Task-Action-Grammar** (TAG), Reisner 1981

- Task[direction,unit]=symbol[direction] + letter[unit] 
- Symbol[direction=forward]=”CTRL”  
- Symbol[direction=backward]=”ALT”  
- Letter[unit=word]=”W”
- Letter[unit=paragraph]=”P”

> [!example] Commands
> - Move cursor on word forward: CTRL-W  
> - Move cursor on word backward: ALT-W  
> - Move cursor on paragraph forward: CTRL-P 
> - Move cursor on paragraph forward: ALT-P

#### Inconsistencies
- Dragging file operations?
	- folder on same disk vs. folder on different disk
	- file to trash can vs. disk to trash can
- [[Fitts' Law]] suggests bigger buttons for more often used operations
- Sometimes inconsistency is wanted
	- E.g. Getting attention for a dangerous operation
	- Consistency on [[#^c37730|semantic level]] may cause inconsistency on [[#^e295fc|syntactic level]]
	- Example:
		- Confirmation of operation is default option
		- Confirmation of reformat command?
### Enable frequent users to use shortcuts
- Improves speed for experienced users
- Shortcuts on different levels
	- Access to single commands, e.g. keyboard shortcuts (CTRL+S) or toolbar
	- Customizing of commands and environments, e.g. printer preset (duplex, A4,...)
	- Reusing actions performed, e.g. history in command lines, macro functionality
- Shortcuts to single commands are related to consistency
	- CTLR+X, CTRL+C, CTRL+V in Microsoft applications for cut, copy and paste
	- However CTRL+S (saving a document) is only implemented in some applications...
### Offer informative feedback
- For any action performed the user should have appropriate and informative feedback
- For frequent actions it should be modest, peripheral
- For frequent action it should be more substantial
### Design dialogues to yield closure
- Sequences of actions should have a beginning, middle and end
- For non-instantaneous actions
- On different levels - E.g.
	- in the large: Web shop - it should be clear when I am in the shop, and when I have successfully checked-out
	- in the small: a progress bar
### Error prevention/handling
- Create UI that makes it hard to make errors
	- menus instead of commands
	- Options instead of text fields
- Detect errors or possible errors
- This rule is related to "easy reversal of actions"
- Make it hard for the user to make errors

> [!example] Detecting errors
> - Leaving an editor without saving
> - Writing to a file that already exists

- Different potions how to handle it:
	- Involve the user (current practice)
	- Prevent the error or its consequences on system level (e.g. create backups/versions when a file is overwritten, keep all files that have been created by the user)
### Permit easy reversal of actions
- As a basic rule - *all actions should be reversible*
- Providing UNDO functions (possibly with infinite depth)
- Allow undo of groups of actions
- Undo is not trivial if user is not going sequential
	- E.g. write a text, copy it into the clipboard, undo the writing
	  -> the text is still in the clipboard!
- Reversal of actions becomes a usage concept
	- Browser back-button is used for navigation (for the user a conceptual reversal of action)
	- Formatting of documents - e.g. "lets see how this looks, ... don't like it, ... go back to the old state"
### Support internal focus of control
- Users should feel to be in control of the system
- User should initiate actions (initiator instead of responder)
- Avoid non-causality
- The system should be predictable
- Some current pervasive computing developments are in contrast:
	- Proactive computing
	- Intelligent agents
	- Smart environments
- Have to be aware when designing these!
- Urban Stress: Experiments on Noise and Social Stressors
- People have to complete tasks under noisy conditions
	- Group A can switch off the noise (remark: if the switch is used, the study results cannot be used)
	- Group B has no influence over the noise
### Reduce short-term memory load
- 7 +/- 2 chunks of information
- The system should remember, not the user

> [!example] Examples that create problems
> - Multi-page forms where the user has to know at form N what she filled in form N-1
> - Abbreviations introduced in one step and used in the following (e.g. user selects a destination - as the name of a city - and the system does the following steps by showing the airport code)

- Helpful
	- Make information that is required visible
	- Use memory aids (visual or audio)
## Prevent Errors

> [!example]- Examples
> - **Correct matching pairs**
> 	- Making some text `<B>bold</B>` will make too much bold if the `</B>` is omitted or mistyped
> 	- IDE often provide {} match checking
> - **Complete sequences**
> 	- Assistance for the user to complete a sequence of actions to perform a task (Wizards)
> - **Command correction**
> 	- Aim: Trying to prevent users entering incorrect commands
> 	- ex. File completion on Unix, Helpful error messages

- Design implications
	- Assume all possible errors will be made
	- Minimize the chance to make errors (constraints)
	- Minimize the effect that errors have (is difficult!)
	- Include mechanism to detect errors
	- Attempt to make actions reversible
### Understanding Errors
- Errors are routinely made  
	- Communication and language is used between people to clarify – more often than one imagines  
	- Common understanding of goals and intentions between people helps to overcome errors

Two fundamental categories
- **Mistakes** = wrong goal
	- overgeneralization 
	- wrong conclusions
- **Slips** = right goal but wrong action
	- Result of “automatic” behavior  
	- Appropriate goal but performance/action is wrong
### Understanding the types of Slips Users Make
- **Capture errors**: Two actions with common start point, the more familiar one captures the unusual (driving to work on Saturday instead of the supermarket)
- **Description errors**: Performing an action that is close to the action that one wanted to perform (putting the cutlery in the bin instead of the sink)
- **Data driven errors**: Using data that is visible in a particular moment instead of the data that is well-known (calling the room number you see instead of the phone number you know by heart)
- **Associate action errors**: You think of something and that influences your action. (e.g. saying come in after picking up the phone)
- **Loss-of-Activation error ~ forgetting**: In a given environment you decided to do something but when leaving then you forgot what you wanted to do. Going back to the start place you remember.
- **Mode error**: You forget that you are in a mode that does not allow a certain action or where a action has a different effect

---
References: