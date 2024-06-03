import List "mo:base/List";
import Nat "mo:base/Nat";

actor StudentProductivityApp {

  // Task Management
  var ListOfTasks = List.nil<Task>();

  type Task = {
    id: Nat;
    description: Text;
    completed: Bool;
  };

  public query func TodaysDate(date: Text): async Text {
    return date;
  };

  public func addTask(description: Text): async Text {
    let newTask: Task = { id = List.size(ListOfTasks) + 1; description = description; completed = false };
    ListOfTasks := List.push<Task>(newTask, ListOfTasks);
    return newTask.description # ", added";
  };

  public query func AllTasks(): async List.List<Task> {
    return ListOfTasks;
  };

  public query func RemoveTasks(): async List.List<Task> {
    let matchedTask = List.filter<Task>(
      ListOfTasks,
      func(task) {
        task.completed == true
      }
    );
    return matchedTask;
  };

  // Function to remove a task by id
  public func removeTask(id: Nat): async Text {
    let newList = List.filter<Task>(
      ListOfTasks,
      func(task) {
        task.id != id
      }
    );
    if (List.size(newList) == List.size(ListOfTasks)) {
      return "No task found with id " # Nat.toText(id);
    } else {
      ListOfTasks := newList;
      return "Successfully removed task with id " # Nat.toText(id);
    }
  };

  // Class Schedule
  type Class = {
    id: Nat;
    name: Text;
    day: Text;
    time: Text;
  };
  
  var classes = List.nil<Class>();

  public query func getClasses(): async List.List<Class> {
    return classes;
  };

  public func addClass(name: Text, day: Text, time: Text): async Text {
    let newClass: Class = { id = List.size(classes) + 1; name = name; day = day; time = time };
    classes := List.push<Class>(newClass, classes);
    return newClass.name # " added to your Schedule";
  };

  // Function to remove a class by id
  public func removeClass(id: Nat): async Text {
    let newList = List.filter<Class>(
      classes,
      func(cls) {
        cls.id != id
      }
    );
    if (List.size(newList) == List.size(classes)) {
      return "No class found with id " # Nat.toText(id);
    } else {
      classes := newList;
      return "Successfully removed class with id " # Nat.toText(id);
    }
  };

  // Flashcards
  type Flashcard = {
    id: Nat;
    question: Text;
    answer: Text;
  };

  var flashcards = List.nil<Flashcard>();

  public query func getFlashcards(): async List.List<Flashcard> {
    return flashcards;
  };

  public func addFlashcard(question: Text, answer: Text): async Text {
    let newFlashcard: Flashcard = { id = List.size(flashcards) + 1; question = question; answer = answer };
    flashcards := List.push<Flashcard>(newFlashcard, flashcards);
    return "Flashcard added: " # question;
  };

  // Function to remove a flashcard by id
  public func removeFlashcard(id: Nat): async Text {
    let newList = List.filter<Flashcard>(
      flashcards,
      func(card) {
        card.id != id
      }
    );
    if (List.size(newList) == List.size(flashcards)) {
      return "No flashcard found with id " # Nat.toText(id);
    } else {
      flashcards := newList;
      return "Successfully removed flashcard with id " # Nat.toText(id);
    }
  };

  // Goal Management
  type Goal = {
    id: Nat;
    description: Text;
    progress: Nat;
    target: Nat;
  };

  var goals = List.nil<Goal>();

  public func addGoal(description: Text, target: Nat): async Text {
    let newGoal: Goal = { id = List.size(goals) + 1; description = description; progress = 0; target = target };
    goals := List.push<Goal>(newGoal, goals);
    return "Goal added: " # description;
  };

  public func updateProgress(goalId: Nat, progress: Nat): async Text {
    let updatedGoals = List.map<Goal, Goal>(
      goals,
      func(goal) {
        if (goal.id == goalId) {
          { id = goal.id; description = goal.description; progress = goal.progress + progress; target = goal.target }
        } else {
          goal
        }
      }
    );
    goals := updatedGoals;
    return "Progress updated for goal with id " # Nat.toText(goalId);
  };

  public query func getGoals(): async List.List<Goal> {
    return goals;
  };
};
