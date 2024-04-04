---
Tags: 
Created: 2024-02-14 13:48:51
---
(Links:: [[Software Design]])
# Description
This project requires the implementation of software for learning a given language using flashcards. The user of this project is a person who needs to learn a new language, e.g., Dutch, interactively. You can see this project as developing an ultra-basic version of apps like Duolingo running locally on the laptop of the user. The system shows flashcards to its users according to levels. Each learning trajectory can be viewed as a workflow composed of steps, each with an associated set of flashcards and basic actions. The set of basic features of the system includes the following: 
1. manage levels: create, update, and delete levels, their (ordered) flashcards, their associated solutions, etc.
2. learning mode: the user can start levels and progress across their corresponding flashcards on a step-by-step basis with gamification elements
3. persistence: levels and flashcards are persisted as JSON files and can be imported/exported from/into a predefined folder
4. provide to the user an overview of their achievement badges based on the performed learning activities. The main technical challenge of this project is to make your system as independent as possible from the specific levels and their corresponding flashcards; in other words, it must be easy to add/configure new levels and flashcards with no changes in the models/code. 

## Ideas for a bonus: 
1. generate a printable document containing all the results obtained by the user while learning a certain language
2. integrate an engine for the semi-automatic generation of new flashcards for a given language
3. integration of a sound module for allowing the user to train also their listening and speaking skills.

```mermaid
---
title: Class Diagram
---

classDiagram

%% Done %%
    FlashCard "1..*" --* "1" Section
    Section "1..*" --* "1" SystemLevel
    Section "1..*" --* "1" UserLevel
    json_Data <.. menu : {Read only}
    Statistics "1" *-- "1" Achievements
    SystemLevel --|> ABSTRACT_Level
    UserLevel --|> ABSTRACT_Level
    LearningMode --|> ABSTRACT_SubMenu
    Achievements --|> ABSTRACT_SubMenu
    LevelEditor --|> ABSTRACT_SubMenu
    JFrame <-- main
    main "1" *-- "0..1" ABSTRACT_SubMenu
    main "1" *-- "0..1" menu
%% Not Done %%
    ABSTRACT_SubMenu --|> menu
    UserLevel --|> LevelEditor
    Section "1" *-- "1" LearningMode
    Statistics <-- LearningMode

%% XOR ABSTRACT SUBMENU AND MENU %%
%% XOR USER_LEVEL AND SYSTEM_LEVEL %%

    class FlashCard{
        -question: string
        -answer: string
        -userResult: boolean
        -attempted: boolean
        +setQuestion(question: string) void
        +setUserResult(userResult: boolean) void
        +setAttempted(attempted: boolean) void
        +setAnswer(answer: boolean) void
        +getAttempted() boolean
        +getQuestion() boolean
        +getUserResult() boolean
        +getAnswer() boolean
    }
    class Section{
        -name: String
        -Flashcards: List<FlashCard>
        +setName(name: string) void
        +moveFlashCards(pos1: int, pos2: int) void
        +addFlashCard(curFlashCard: FlashCard) void
        +deleteFlashCard(pos: int) void
        +getFlashCard(pos: int) FlashCard
    }
    class SystemLevel{
        -difficulty: int
        +setDifficulty(difficulty: int) void
        +getDifficulty() int
    }
    class UserLevel{
        
    }
    class ABSTRACT_Level{
        -name: string
        -SectionList: List<Section>
        +setName(name: string) void
        +addSection(curSection: Section) void
        +moveSection(pos1: int, pos2: int) void
        +removeSection(pos: int) void
        +editSection(pos: int) void
        +getSection(pos: int) Section
        +getName() string
    }
    class json_Data{

    }

    class menu{
        -exit: ActionListener
        -LevelList: List<Level>
        -updateMenu() void
        -drawMenu() void
        +displayLevels() void
        +displaySections(curlevel: Level) void
        +startLevelEditor() void
        +startAchievements() void
        +startLearningMode(curSection: Section) void
    }
    class ABSTRACT_ SubMenu{
        -exit: ActionListener
        -updateSubMenu() void
        -drawSubMenu() void
        -leaveSubMenu() void
    }
    class LearningMode{
        -curAnswer: string
        -updateStatistics() void
        +checkAnswer(curAnswer: string) void
        +nextFlashCard() void
    }
    class Achievements{
		-progress: int
		-target: int
		+updateProgress(progress: int) void
    }
    class LevelEditor{
        +addLevel(curLevel: Level) void
        +moveLevel(pos1: int, pos2: int) void

    }
    class Statistics{
        -totalXP: int
        -totalLevels: int
        -streak: int
        +addXP() void
        +getXP() int
        +incLevels() void
        +getLevels() int
        +incStreak() void
        +getStreak() int
    }
    class JFrame{
    }
    class main{
	    -frame: JFrame
	    -exit: ActionListener
	    +exit() void
		-initProgram() void
		-runMenu() void
		-runLevelEditor() void
		-runAchievements() void
		-runLearningMode() void
		+main() void
    }
```

---
References: