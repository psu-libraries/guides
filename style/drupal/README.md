This is meant to be a clearinghouse for all you need to know for local development collaborative work with Drupal:

1. Development
    1. Guiding Principles
    1. Code Repository
    1. Module Library
    1. Issue Tracking
    1. Environment and Workflow
    1. Deployment
    1. Coding Standards
    1. File Organization
    1. Formatting
1. Specific Drupal Site Building and Development Guidelines
    1. Drupal System and Sub-Systems
    1. Feature Modules

## Development

### Guiding Principles

* Do it the "The Drupal Way"
* Don't hack core or contrib.‡
* Override, override, override
* Separate logic from theming
* Be leery of over-engineering
* KISS over DRY
* Code should be self-documenting
* Automate trivial, monotonous tasks
* Keep Drupal as light as possible
* No orphans
* Don't touch the MySQL directly (or exercise extreme caution when doing so)

All code commits to the UL, staff and policy projects* will be reviewed by the Lead Web Developer. Code commits in other projects do not necessarily need to be reviewed but may be.  In general, anyone on the team can request that any other member of the team review code, but again, in the UL, staff and policy projects all code *must* be reviewed and ultimately approved or rejected by the Lead Web Developer. Please see [Development Communications Best Practices](https://staff.libraries.psu.edu/libraries-strategic-technologies/daws/development/development-communications-best-practices) for information on how we communicate around moving code.

*A project is any Drupal-based code repository on git.psu.edu in the I-Tech group.

### Code Repository

* We are using git to manage our code, and using git.psu.edu to act as our remote repository service. git allows us to work on projects together without overwriting each others' work and provides us with a history of all code that has been written and changed.
* Code is versioned by "commits" or a group of related changes to code. Commits should be more than glorified save points. They should be discrete or atomic additions or modifications of functionality. Avoid aggregating a multitude of unrelated modifications.
* All commits should have brief messages that reflect the nature of the change. Try to succinctly emulate what would be the explanation during a code review.
* See this post for how to write ideal commit messages: http://chromaticsites.com/blog/how-write-great-commit-message
* SourceTree is a great GUI for Git that can make atomic commits trivially easy.
* The Staff site and the Public site have protected branches for master and qa. Code is merged in by the Lead Web Developer only.

### Module Library

The Lead Web Developer will review and approve all modules used, in any project.

### Issue Tracking

Use GitLab's issue tracking system to track bugs. And follow these guidelines.

### Environment and Workflow

The flow for ALL of our Drupal work is the following: Local Dev › QA › Production

We're using the [DrupalVM](https://www.drupalvm.com/) project as the base for our local development:

* See [our local implementation and instructions for getting started](https://git.psu.edu/i-tech/psulib-drupal-vm)
* See [more notes about our development workflow in the context of DrupalVM](https://staff.libraries.psu.edu/libraries-technology-i-tech/daws/local-development-and-drupalvm).
* Local will be the place where we hack, play and polish code. See detailed notes on the way in which we use [git flow](https://staff.libraries.psu.edu/libraries-strategic-technologies/daws/development/git-flow-collaborating-git-and-drupal), Collaborating on git and Drupal.
* For a bigger picture, see [Drupal development workflow visualization](https://staff.libraries.psu.edu/sites/default/files/restricted/PennStateLibrariesDrupalDevWorkflow%20%281%29_0.pdf)
* See documentation on [how to work with Vagrant](https://staff.libraries.psu.edu/libraries-technology-i-tech/daws/vagrant-notes).

Note: For microsites, and other single developer projects, multiple branching is not a requirement (all work can be done on master) 

### Deployment

All projects will deploy on demand as needed, not via a weekly window of opportunity as had been the rule in Adobe CQ.

### Coding Standards

It is important to strike the right balance between systems building and pragmatism. This is a nice quote for summing up our approach to development. In other words, coding is an art and a science.

    Sometimes, it’s better to repeat a little to keep the code maintainable, rather than building a top-heavy, unwieldy, unnecessarily complicated system that is completely unmaintainable because it is overly complex... ...pragmatism trumps perfection. At some point, you will probably find yourself going against the rules described here. If it makes sense, if it feels right, do it. Code is just a means, not an end.

-Hugo Geraudel [Sass Guidelines](http://sass-guidelin.es/#key-principles)

Whenever developing significant adavanced functionality with PHP, we will use object-oriented programming in tandem with functional programming approaches to avoid "code spaghetti" -- or code that is difficult to read, maintain and troubleshoot. An example would be deeply nested foreach and if/else loops. With PHP we'll ascribe to PSR-1 as much as possible (except where it conflicts with Drupal 7 standards, and in those cases D7 standards win out).

### File Organization

* We do not track any code we don't modify or write ourelves. Thus, repos should never contain any core or contrib code with the exception of settings.php and .htaccess. Settings.php will always include an include at the end for a "local.settings.php" that will contain sensitive information and will not be tracked in repositories, ever.
* Put code that is solely front-end oriented in the theme.
* Put code that is solely back-end oriented in a custom module.
* Modules should be organized by sub-system OR type. Examples:
    * Sub-system: All of the hooks, custom PHP, CSS, JavaScript, templating, etc. required to retrieve tech lending device availability data and display it to the end user.
    * Type: There are many common hooks that get used per content-type, view, service, etc. Things that you use to swoop in and tamper with before Drupal sends something to the user. For the sake of sanity, it's best to keep this all in one place. Some examples: form alterations, service alterations, feed modifications, etc.
* If there's custom code associated with a Feature, just put that code in the feature's .module file.

### Formatting

* Use an indent of 2 spaces, with no tabs (regardless of file type).
* No trailing whitespace at the end of lines
* 80 characters wide lines (as much as possible)
* Unix line endings, not Windows line endings
* Files should be formatted with \n as the line ending (*Unix line endings*), not \r\n (Windows line endings).
* All text files should end in a single newline (\n). This avoids the verbose "\ No newline at end of file" patch warning and makes patches easier to read since it's clearer what is being changed when lines are added to the end of a file.

Way more on the subject of coding standards can be found at the [Drupal.org Coding Standards](https://www.drupal.org/docs/develop/standards/coding-standards) page. It is highly recommended to [install CodeSniffer with Drupal standards(https://www.drupal.org/node/1419988)] baked in.

[Coder Module](https://www.drupal.org/project/coder) can help with this too, think of it as a linter. Installing PHP CodeSniffer via the Coder module is a great idea too. It can be [installed and run with Drupal standards as the ruler](https://www.drupal.org/node/1419988). Jenkins will be testing for this.

For our Sass conventions, let's try to stick to the rules listed at http://sass-guidelin.es

## Specific Drupal Site Building and Development Guidelines

### Drupal System and Sub-Systems

The following should be some questions we ask ourselves when we create something in Drupal. The idea is to manage our content management system. We don't want any orphans just hanging out taking up space, or worse, bottlenecking.

### Modules

Modules are a huge part of Drupal. We will need to enable dozens of core and contributed modules in order to produce our sites. In the best cases, a module can save you the time of having to write functionality yourself and will make complex functionality simple. The modules we pick should take into account things like whether or not the module is under active development on d.o, how many downloads there have been and what the code looks like.

Sometimes writing our own custom module will be the best way to go, especially for things like tampering with the content being delivered to a page (like a view, service, node, etc). And sometimes it may make sense to simply grab a small part of code from another contrib module and use it for our own purposes without grabbing all of the entire module. All of this will be handled on a case-by-case basis as context is going to make a big difference.

### Views, Roles, Blocks, Content Types, Taxonomy

These items, and some others, are things in Drupal that can be created programmatically but are usually created through the admin interface. We'll also be utilizing the Features module (and the Strongarm module) to save much of what is done in the admin interface to code.

With that said, we need to document these things in Drupal when we create them. To name things, just use human friendly text, like capitalizing first words and using spaces instead of dashes and underscores.

### Content Types

Here are some thoughts on when to elevate something to a Content Type.

1. Is this thing going to have more than one field?
1. Is this thing going to have more than what is already present in the default Page Content Type?
1. Do multiple people need to be involved in creating, reviewing, or managing this content?
1. Does your content need to have any workflow around the creation, editing or management of it?
1. Will your content need to have an associated metadata (for SEO or content portability)?
1. Will you need to control who has access to your content in its entirety or certain elements of it?

If the answer is "no" to all of these, then this doesn't need to be a new Content Type.

If the answer is "yes," consider the following:

    Do you plan on using this content in multiple Views throughout the site?
    Is there a lot of content?

### Taxonomy

In general we're probably going to want to avoid using taxonomies.  They don't have all the same features of a true node entity in Views, which is really limiting.  Taxonomies give you hierarchy and are really useful for classifying.  Example: "Subject"

Keep in mind you can add fields to it, like image, text, etc.

### Custom Entities

Some of the data objects within Drupal core - content types, users, and taxonomies - are entities. These entities can meet the majority of the data needs of most clients, but there are some cases where you need a custom entity to represent data you want to display to end users. In general custom entities represent a kind of complexity that fails the KISS test, but in certain case we might want to explore them. Here are some questions to consider:

1. Is your data representative of something that is not content?
1. Is it the case that you don't need any of Drupal's workflows or editing tools for your content?
1. Are you trying to hide your content from search results?
1. Are the interactions with this content completed through a very customized interface, like a game?

Some of the above is influenced or taken from this [blogpost](https://www.4sitestudios.com/blog/inventing-the-future-of-websites). In general working with Custom Entities is complicated and should probably only be utilized when all other options have been taken off the table.

##Menus

1. Main navigation: Drupal menus controlled by admins (us)
1. Sub-menu navigation: Drupal menus controlled by staff/editors
1. Footer menus: Developer's choice, but preferred method is hard coding these in the theme template files.

### Feature Modules

Feature modules should be used as much as possible for the sake of code history, performance and code safety (i.e., falling back to a working revision). The Strongarm module must be used as well, which allows you to grab deeper items in Drupal and write them to code.

Feature modules should be relatively small. For example, everything that a given content type might require in order to deliver results to the user: the definition of the content type, associated views, dependent modules, image styles, etc. Features that grow too large can become unwieldy.

Take a look at this [blogpost](http://befused.com/drupal/drush-features) to see a full list of commands you can make with drush for features along with explanations of why and when to use those commands. 

‡ If you need to hack core/contrib, this should be reviewed and signed off on by others first. And the hacks will be stored in a code repository as a .patch and a copy of the module fork.
