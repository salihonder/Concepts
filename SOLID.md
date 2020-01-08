# SOLID
SOLID Principles is a coding standard that all developers should have a clear concept for developing software in a proper way to avoid a bad design.

## S — Single responsibility principle
In programming, the Single Responsibility Principle states that every module or class should have responsibility over a single part of the functionality provided by the software.

In the article Principles of Object Oriented Design, Robert C. Martin defines a responsibility as a ‘reason to change’, and concludes that a class or module should have one, and only one, reason to be changed.

`Violation Example`
```c#
// It is a vialotion since CreatePost does three major things
// add post to DB and Add log error to db also writing to a local file

class User
{
    void CreatePost(Database db, string postMessage)
    {
        try
        {
            db.Add(postMessage);
        }
        catch (Exception ex)
        {
            db.LogError("An error occured: ", ex.ToString());
            File.WriteAllText("\LocalErrors.txt", ex.ToString());
        }
    }
}
```

`Correct Example`
```c#
// Now we have two classes that each has one responsibility; to create a 
// post and to log an error, respectively.
class Post
{
    private ErrorLogger errorLogger = new ErrorLogger();

    void CreatePost(Database db, string postMessage)
    {
        try
        {
            db.Add(postMessage);
        }
        catch (Exception ex)
        {
            errorLogger.log(ex.ToString())
        }
    }
}

class ErrorLogger
{
    void log(string error)
    {
      db.LogError("An error occured: ", error);
      File.WriteAllText("\LocalErrors.txt", error);
    }
}
```

## O — Open/closed principle

In programming, the open/closed principle states that software entities (classes, modules, functions, etc.) should be open for extensions, but closed for modification.

`Violation Example`
```c#
// what if we need another option like starting with @
// we need to modify the class. It is a violation of Open/closed principle
class Post
{
    void CreatePost(Database db, string postMessage)
    {
        if (postMessage.StartsWith("#"))
        {
            db.AddAsTag(postMessage);
        }
        else
        {
            db.Add(postMessage);
        }
    }
}
```


`Correct Example`
```c#
// By using inheritance, it is now much easier to create extended behavior /// to the Post object by overriding the CreatePost() method.
class Post
{
    void CreatePost(Database db, string postMessage)
    {
        db.Add(postMessage);
    }
}

class TagPost : Post
{
    override void CreatePost(Database db, string postMessage)
    {
        db.AddAsTag(postMessage);
    }
}
```


## L — Liskov substitution principle
It states that if S is a subtype of T, then objects of type T may be replaced (or substituted) with objects of type S.

`Violation Example`
```c#
/* Observe how the call of CreatePost() in the case of a subtype 
MentionPost won’t do what it is supposed to do; notify the user and 
override existing mention.
Since the CreatePost() method is not overridden in MentionPost the CreatePost() call will simply be delegated upwards in the class hierarchy and call CreatePost() from its parent class.
*/

class Post
{
    void CreatePost(Database db, string postMessage)
    {
        db.Add(postMessage);
    }
}

class TagPost : Post
{
    override void CreatePost(Database db, string postMessage)
    {
        db.AddAsTag(postMessage);
    }
}

class MentionPost : Post
{
    void CreateMentionPost(Database db, string postMessage)
    {
        string user = postMessage.parseUser();

        db.NotifyUser(user);
        db.OverrideExistingMention(user, postMessage);
        base.CreatePost(db, postMessage);
    }
}

class PostHandler
{
    private database = new Database();

    void HandleNewPosts() {
        List<string> newPosts = database.getUnhandledPostsMessages();

        foreach (string postMessage in newPosts)
        {
            Post post;

            if (postMessage.StartsWith("#"))
            {
                post = new TagPost();
            }
            else if (postMessage.StartsWith("@"))
            {
                post = new MentionPost();
            }
            else {
                post = new Post();
            }

            post.CreatePost(database, postMessage);
        }
    }
}
```

`Correct Example`

```c#
// By refactoring the MentionPost class such that we override the 
// CreatePost() method rather than calling it on its base class, 
// we no longer violate the Liskov substitution principle.

class MentionPost : Post
{
    override void CreatePost(Database db, string postMessage)
    {
        string user = postMessage.parseUser();

        NotifyUser(user);
        OverrideExistingMention(user, postMessage)
        base.CreatePost(db, postMessage);
    }

    private void NotifyUser(string user)
    {
        db.NotifyUser(user);
    }

    private void OverrideExistingMention(string user, string postMessage)
    {
        db.OverrideExistingMention(_user, postMessage);
    }
}
```

## I — Interface segregation principle

It states that no client should be forced to depend on methods it does not use.

Put more simply: Do not add additional functionality to an existing interface by adding new methods.
Instead, create a new interface and let your class implement multiple interfaces if needed.

`Violation Example`
```c#
/*
In this example, let’s pretend that I first have an IPost interface with the signature of a CreatePost() method.
Later on, I modify this interface by adding a new method ReadPost(), so it becomes like the IPostNew interface.
*/


interface IPost
{
    void CreatePost();
}

interface IPostNew
{
    void CreatePost();
    void ReadPost();
}
```

`Correct Example`

```c#
interface IPostCreate
{
    void CreatePost();
}

interface IPostRead
{
    void ReadPost();
}

```

## D - Dependency inversion principle

This principle is a way to decouple software modules.
This principle states that
- High-level modules should not depend on low-level modules. Both should depend on abstractions.
- Abstractions should not depend on details. Details should depend on abstractions.

To comply with this principle, we need to use a design pattern known as a dependency inversion pattern, most often solved by using dependency injection.
Dependency injection is a huge topic and can be as complicated or simple as one might see the need for.
Typically, dependency injection is used simply by ‘injecting’ any dependencies of a class through the class’ constructor as an input parameter.

`Violation Example`
```c#
// Observe how we create the ErrorLogger instance from 
// within the Post class.
This is a violation of the dependency inversion principle.
class Post
{
    private ErrorLogger errorLogger = new ErrorLogger();

    void CreatePost(Database db, string postMessage)
    {
        try
        {
            db.Add(postMessage);
        }
        catch (Exception ex)
        {
            errorLogger.log(ex.ToString())
        }
    }
}

```

`Correct Example`

```c#
class Post
{
    private Logger _logger;

    public Post(Logger injectedLogger)
    {
        _logger = injectedLogger;
    }

    void CreatePost(Database db, string postMessage)
    {
        try
        {
            db.Add(postMessage);
        }
        catch (Exception ex)
        {
            logger.log(ex.ToString())
        }
    }
}
```