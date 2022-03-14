Blazing Realms: Space - Game Client
======================================

Main Resources
------------------------
* [Documentation](https://github.com/brinca/blazing-realms-space-client/wiki)
* [Tasks](https://github.com/brinca/blazing-realms-space-client/issues)
* [Planning](https://github.com/brinca/blazing-realms-space-client/projects/1?add_cards_query=is%3Aopen)

Usage/Build instructions
------------------------
1. Install [Git-LFS](https://git-lfs.github.com/)
2. Make sure you have installed [Unity 2021.2.x](https://unity3d.com/get-unity/download/archive) with (at least) the WebGL and Android targets
3. Open the project in the Unity editor
4. Select `File > Build and Run`

Recommendations (optional)
--------------------------
* A nice (free) visual client is [SourceTree](https://www.sourcetreeapp.com/) (supports Git Flow)
* Check out a nice [HowTo on Git-LFS for Unity](http://en.joysword.com/posts/2016/03/setting_up_github_for_unity_projects/)

Coding Style
------------
* [Naming guidelines](https://msdn.microsoft.com/en-us/library/ms229002(v=vs.110).aspx)
* Use PascalCase for member names (including private), and camelCase for local variables
* Avoid using prefixes (e.g. m_SomeName), except when necessary (e.g. private field for a getter/setter, etc)
* Use "On" prefixes only for event handlers, but not the event declarations, as these should reflect an action that has happened or is about to happen (i.e. instead of `OnMouseClick`, use `MouseClicked`) 
* Prefix interfaces with I (e.g. ICullable, IGameplayService, etc)
* Postfix services and controllers (screens, views, etc) with the corresponding type (e.g. SplashScreen, UserListingView, GameplayService, etc)
* Always enclose code in the `danielbrinca` namespace unless you want to isolate a certain feature (e.g. `danielbrinca.experimental`)
* Always document public classes/members using [xml syntax](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/xmldoc/recommended-tags), with at least _summary_, _param_ and _returns_ tags (where applicable) 

Git strategy
------------
* Follow the [Git Flow](http://nvie.com/posts/a-successful-git-branching-model/) branching strategy
* To make any change:
  * Create a ticket describing the change
  * Branch off `develop`, following the branch naming convention (below)
  * After commiting your changes, push your branch to origin, and open a Pull Request on Github
  * Ask a lead to review and merge your PR
* Branch naming convention:
  * Features: type/xxx-succinct-description (replace xxx with the related ticket id, and type with `feature`, `fix`, `refactor`, `test`, etc)
  * Release: release/x.x (x.x is the new release version)
  * Hotfix: hotfix/x.x.x (x.x.x is the hotfix version)
* Commit writing convention:
  * Mention the corresponding ticket issue by including the id at the start of the first line (e.g. #77)
  * The first line should describe, in a very succinct manner, the subject of the changes (e.g. "#77: Implement friends list")
  * Optionally you can add a more thorough description of the changes (lists can be bulleted by asterisks), separated by a blank line from the first line
* Feature should be removed (at least on origin) after merging
* Old release/hotfix branches should be removed only after the next release/hotfix branch is merged
* Squash commits when merging to dev/release (unless there's a good reason not to)
* All feature merges must be code reviewed by a colleague or the lead
* All dev/release/hotfix merges must be handled by the lead/owner

Release strategy
----------------
* All features are to be developed in their own branches, and eventually merged with the `develop` branch, as detailed above
* Once there are enough features in `develop` to create a new release:
  * A new `release` branch should be created from `develop`, following the above naming convention
  * Any last minute changes, bug-fixing, etc, should be merged to the `release` branch
  * Once fully tested, the `release` branch should be merged to `master`, and then back to `develop`
* The merge commit to `master` should be tagged with the appropriate release version number (e.g. "0.1.2")

Hotfix strategy
---------------
* If any game-breaking bugs surface once the build is released to the general public, a hotfix branch may be created to address them
* In this case, a `hotfix` branch is created off of the `master` branch, using the above naming conventions
* Once the problem is solved, the `hotfix` branch is merged back to `master`, and then back to `develop`
* The merge commit to `master` should be tagged with a new release version number (e.g. "0.1.2.1")
* Once merged, the old `release` branch can be removed and the new `hotfix` branch becomes the latest _release_ branch 
* Please note that `release` and `hotfix` branches are functionally equivalent, since both represent new releases (though for different reasons)

Deployment strategy
-------------------
* Merges against `master` or `develop` will trigger the CI/CD system to create new builds for all targeted platforms
* Once the builds are available, they will be manually uploaded to the `internal` track for each platform
* If the builds are meant to be released to the public:
  * Upon successful QA testing in the `internal` track, the builds are promoted to the `testing` track (i.e. available to beta testers) for further testing and feedback
  * Once all testing/feedback from the `testing` track is processed, then the builds are promoted to the `production` track (i.e. available to the general public)
* To note that "beta-testers" in this context means regular players that have opted-in to the `testing` branch (but are not necessarily members of the dev team)
* The deployment tracks (`internal`, `testing`, `production`, etc) may have different names under different platforms