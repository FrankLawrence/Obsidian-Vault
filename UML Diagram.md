```mermaid
sequenceDiagram
    User->>Main: runLevelEditor()
    Main->>AppUI: switchView(LevelHandler)
    AppUI->>User: viewChanged

	alt input=Create
		User->>ManagerLogic: createLevel(name, hardness)
		ManagerLogic->>UserLevel: addSection()
		User->>ManagerLogic: input(name)
		ManagerLogic->>UserLevel: setName(name)
		loop
			User ->>ManagerLogic: input(question, answer)
			ManagerLogic->>Section: addCard(Card)
		end
	else input=Delete
		User->>ManagerLogic: deleteLevel(name)
	else input=update
		User->>ManagerLogic: updateLevel(name)
	end
		
```
```mermaid
classDiagram
	direction TB
	Card --* Section
	Section --* Level
	Main *-- Menu
	Menu *-- PracticeLogic
	Menu *-- ManagerLogic
	States <-- Achievement
	PracticeLogic "1" --> "1" Statistics : updates
	Statistics "1" --> "1..*" Achievement
	PracticeLogic "1" --> "1..*" Section : reads
	ManagerLogic "1" --> "0..*" UserLevel : creates
	Main "1" --> "1..*" SystemLevel : creates

	namespace logic {
		class Main {
			-frame: JFrame
			+exitApp() void
			-initMenu() void
			
			-runLevelEditor() void
			-runAcheivements() void
			-runLearningMode() void
			+main() void
		}

		class Menu {
			-currentLanguage: String
			-userLevels: List< Level >
			-systemLevels: List< Level >
			+getLevels() List< Level >
			+switchLanguage(language: String) void
		}
		
		class PracticeLogic {
			-hasPracticed: boolean
			+currentFlashCard: Card
			+nextFlashCard() Card
			+addPoints(points: int) void
			+increaseStreak() void
		}
		
		class ManagerLogic {
			
		}
	}
	
	Level <|-- UserLevel
	Level <|-- SystemLevel

	namespace entities {
		class Card {
			-solution: String
			-question: String
			-points: int
			+mark(correct: boolean) void
			+getResult() String
			+setSolution(sol: String) void
			+setQuestion(que: String) void
		}
		class Section {
			-name: String
			-editable: boolean
			-cards: List< Card >
			-attempts: int
			-totalCorrect: int
			%%+practice() void%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
			+givePoints(amount: int) void
			+getErrorRate() float
			+getCards() List< Card >
			+addCards(List< Card >) void
			+editCards(List< Card >) void
			-getPoints() int
		}
		class Level {
			<<abstract>>
			-name: String
			-sections: List< Section >
			-hardness: (String, int)
			+getSections() List< Sections >
			+addSection(name: Section) void
		}
	
		class UserLevel {
		}
		class SystemLevel {
			-locked: boolean
		}
	}
	
	namespace UserData {
		class Achievement {
			/state: State
			-amountComplete: int
			-pointsNeeded: int
			+updatePoints(int) void
			-updateStatus(newState: Status) void
		}
		class Statistics {
			-achievements: List< Achievement >
			+addAchievement(newAch: Achievement) void
		}
		class States {
			<<enumerate>>
			LOCKED
			WORKING
			COMPLETE
		}
	}
	
	Main --> AppUI
	AppUI *-- EditorUIStyle
	AppUI *-- MenuUIStyle
	AppUI *-- LearningUIStyle
	EditorUIStyle "1" --> "1" Theme : reads
	MenuUIStyle "1" --> "1" Theme : reads
	LearningUIStyle "1" --> "1" Theme : reads
	namespace graphics {
		class AppUI {
			-currentView: UI
			+updateUI() void
			-switchView(view: UI) void
		}
		class Theme {
		
		}
		class EditorUIStyle {
		
		}
		class MenuUIStyle {
		
		}
		class LearningUIStyle {
		
		}
	}
	
```
# UserData

# Graphics
- **AppUI**: Manages the rendering of the different types of views
	- *currentView*: Can be either of the three UI styles described below. 
	- *updateUI*: 
	- *switchView*
- **Theme**: Used to decide on which colors to use for different UI elements
- **EditorUIStyle**: Describes the UI design of the Editor view, including the button layout, text fields for user input, and smaller things (such as font and colors).
- **MenuUIStyle**: Similar to EditorUIStyle, describes the layout of the elements (buttons and decoration), and styling (font and color) for the Achievements and Level picker menus.
- **LearningUIStyle**: Describes how the cards will appear, and other elements required when practicing/learning flashcards.

We might decide to use a dedicated class for handling font, if we want more customization.