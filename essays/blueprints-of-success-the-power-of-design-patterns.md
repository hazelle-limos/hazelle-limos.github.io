---
layout: essay
type: essay
title: "Blueprints of Success: The Power of Design Patterns"
# All dates must be YYYY-MM-DD format!
date: 2024-04-25
published: true
labels:
  - Software Engineering
  - Design Patterns
---
### Building Blocks
Have you ever built a Lego set? If so, you are most likely familiar with the process: follow the instruction manual and assemble the pieces together. Sounds simple, right? Well, imagine building a large scale Lego set, perhaps one that comes with hundreds, if not thousands, of small pieces. It is possible to take caution and slowly work your way through the set, but what happens if you make mistakes that may go unnoticed? A single block misplaced by just one [stud](https://brickipedia.fandom.com/wiki/Stud#:~:text=Studs%20are%20small%2C%20cylindrical%20bumps,still%20bear%20the%20word%20LEGO), or a piece going missing--a crucial one even--could throw your entire build off. You would then have to backtrack and see where you went wrong. What could you have done to avoid this?

First, let's take a look at how Lego pieces and design patterns share similarities that illustrate their roles as foundational elements:
1. **Similarities**: Just as Lego sets consist of standardized pieces with specific shapes and sizes that fit together seamlessly, design patterns provide standardized solutions to recurring software design problems. This standardization ensures consistency and facilitates ease of use across different projects and teams.
2. **Modularity**: Lego sets encourage modular construction, allowing for the creation of complex structures by combining smaller components. Through design patterns, developers can break down complex systems into smaller, manageable units that can be easily reused and combined to build larger, more sophisticated applications.
3. **Reusability**: Lego bricks are designed to be reusable, allowing builders to dismantle and reconstruct their creations multiple times without losing their structural integrity. Similarly, design patterns promote code reusability by encapsulating common design solutions that can be applied across multiple projects.

So, to answer the question, there are different ways you could have approached the Lego build. The pieces could have been sorted by size, shape, or color--especially those that appear only once throughout the entire set. Alternatively, instead of placing the pieces into one single module, you could perhaps split the build into smaller modules. That way, you would not have to take apart the build to undo a mistake. Additionally, if you conveniently have spare pieces, those can be reused to replace the missing pieces. Finally, make sure to double-check, or even triple-check, with the instruction manual. The documentation is there for a reason.

### Utilizing Design Patterns
As part of our final project for ICS 314, my team and I have worked with several design patterns.

#### Singleton Pattern
Notably, the [Singleton pattern](https://www.patterns.dev/vanilla/singleton-pattern) is implemented through our Mongo Collections. An example from our project is the `ProfilesCollection`: 

```
class ProfilesCollection {
  constructor() {
    // The name of this collection.
    this.name = 'ProfilesCollection';
    // Define the Mongo collection.
    this.collection = new Mongo.Collection(this.name);
    // Define the structure of each document in the collection.
    this.schema = new SimpleSchema({
      firstName: String,
      lastName: String,
      address: String,
      gradYear: String,
      major: String,
      position: {
        type: String,
        allowedValues: ['Mentor', 'Student'],
      },
      prefer: {
        type: String,
        allowedValues: ['Online', 'In-Person', 'Online/In-Person'],
      },
      image: String,
      description: String,
      owner: String,
    });
    // Attach the schema to the collection, so all attempts to insert a document are checked against schema.
    this.collection.attachSchema(this.schema);
    // Define names for publications and subscriptions
    this.userPublicationName = `${this.name}.publication.user`;
    this.adminPublicationName = `${this.name}.publication.admin`;
  }
}
```
This Collection is used to store the data needed to compile a user profile. As we are working with information regarding college students and mentors, we are able to easily access such data as it remains consistent across the application.


#### Observer Pattern
Here, the [Observer Pattern](https://www.patterns.dev/vanilla/observer-pattern) is implemented through the subscription to the `ProfilesCollection`.
```
const ListProfiles = () => {
  const { ready, profiles } = useTracker(() => {
    const subscription = Meteor.subscribe(Profiles.userPublicationName);
    const rdy = subscription.ready();
    const profileItems = Profiles.collection.find({}).fetch();
    return {
      profiles: profileItems,
      ready: rdy,
    };
  }, []);
```
The `useTracker` hook is used to track changes to the data source, which would be the `ProfilesCollection`. The function is subscribed to those changes, as well, and it updates and re-renders the component whenever the data is modified. 

### It Will be Worth It
Revisiting the Lego analogy, it remains similar to design patterns and possibly other processes of software engineering. There will always be simple projects you can work on, but once you tackle one that is more complex and time-consuming, you will have to rethink your strategies. Thus, it is best to adhere to known, proven solutions, as it can save you time when encountering challenges.

As a final note, being formally introduced to design patterns has been an enlightening experience. There are many patterns to learn about, and it will take time to solidify my understanding of them all, but it will be worth it in the end. Design patterns are essential for software development, as it ensures seamless collaboration and efficiency among fellow developers.

(ChatGPT was used to brainstorm essay and header titles, along with guiding essay direction).