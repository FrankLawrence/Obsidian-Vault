@startuml
namespace logic {
    class Main {
        -frame: JFrame
        -systemLevels: List<Systemlevel>
        +exitApp() void
        -initMenu() void
        -runLevelEditor() void
        -runAcheivements() void
        -runLearningMode() void
        +main() void
    }

    class Menu {
        -currentLanguage: String
        -userLevels: List<Level>
        -systemLevels: List<Level>
        -currentLevelType: String
        +displayLevels() void
        -getLevels() List<Level>
        +switchLanguage(language: String) void
        +switchLevelType() void
    }

    class PracticeLogic {
        +displayFlashcard() void
        +currentFlashcard: Flashcard
        +nextFlashcard() Flashcard
        +addPoints(points: int) void
        +increaseStreak() void
        +closePraticeMode() boolean
        -canExit() boolean
    }

    class ManagerLogic {
        -userLevels: List<UserLevel>
        +readJson(): List<UserLevel>
        +addLevel(level: UserLevel) void
        -writeJson(levels: List<UserLevel>) void
    }
    class UserLogic {
        +displayStatistics() void
        +displayAchievements() void
    }
}

namespace entities {
    class Flashcard {
        -hasPracticed: boolean
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
        -attempts: int
        -totalCorrect: int
        +givePoints(amount: int) void
        +getErrorRate() float
        +getFlashcards() List<Flashcard>
        +addFlashcards(List<Flashcard>) void
        +editFlashcards(List<Flashcard>) void
        -getPoints() int
    }

    abstract class Level {
        -name: String
        -hardness: (String, int)
        +getSections() List<Section>
        +addSection(name: Section) void
    }

    Level <|-- UserLevel
    Level <|-- SystemLevel

    class UserLevel {

    }

    class SystemLevel {
        -locked: boolean
        +addPoints(points: int) void
    }
}

namespace user_data {
    class Achievement {
        /state: State
        -amountComplete: int
        -pointsNeeded: int
        +updatePoints(int) void
        -updateStatus(newState: Status) void
    }

    class Statistics {
        -totalXP: int
        -totalLevels: int
        -streak: int
        +incStreak() void
        +resetStreak() void
        +addXP(value: int) void
        +incLevel() void
        -achievements: List<Achievement>
        +addAchievement(newAch: Achievement) void
    }

    enum State {
        LOCKED
        WORKING
        COMPLETE
    }
}

Main "1" *-- "1" Menu
Flashcard "1..*" --* "1" Section
Section "1..*" --* "1" Level
Main "1" *-- "1" PracticeLogic
Main "1" *-- "1" ManagerLogic
PracticeLogic "1" --> "1" Statistics : updates
Statistics "1" --> "1..*" Achievement
Achievement "1" --> "1" State
PracticeLogic "1" --> "1..*" Section : reads
ManagerLogic "1" --> "0..*" UserLevel : creates
UserLogic "1" --> "1" Statistics : reads
Main "1" --> "1..*" SystemLevel : creates
Menu "1" --> "1..*" Level : reads
Menu "1" --> "1" ManagerLogic : reads
Main "1" *-- "1" UserLogic


namespace graphics {
    class AppUI {
        -currentView: UIStyle
        +updateUI() void
        -switchView(view: UIStyle) void
    }

    abstract class UIStyle
    class Theme
    class EditorUIStyle
    class MenuUIStyle
    class LearningUIStyle
    class AchievementUIStyle
}

Main "1" --> "1" AppUI : reloads
AppUI "1" *-- "1" UIStyle
UIStyle <|-- EditorUIStyle
UIStyle <|-- MenuUIStyle
UIStyle <|-- LearningUIStyle
UIStyle <|-- AchievementUIStyle
EditorUIStyle "1" --> "1" Theme : reads
MenuUIStyle "1" --> "1" Theme : reads
LearningUIStyle "1" --> "1" Theme : reads
AchievementUIStyle "1" --> "1" Theme : reads
@enduml