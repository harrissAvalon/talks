<!-- background: #fff -->
<!-- color: #000 -->
<!-- font: frutiger -->

# "Overkilling" Your Git Repo with Gitflow
by Stephon Harris

<script>
  Hi! I'm Stephon out of the DC Office and today I'm going to tell you abut managing your source-controlled workflow with Gitflow.
</script>
***
# Bad workflows need to die
![bad-git](http://i.giphy.com/ToMjGpIYtgvMP38WTFC.gif) ![bad](https://pbs.twimg.com/media/CaI8xA3W0AA6wMe.png)
You need a branching strategy to give your projects life  

<script>
  Managing the flow of code can grow scary using Git when you have a plethora of branches and convoluted merges. Gitflow is a branching model that is collaborative, scalable, and sustainable.
</script>
***
# Wait, But Why?
- Organize code for development and production-ready stages
- Provide workflow for integrating changes
- Parallel development of features
- Continuously deliver through releases and hotfixes
<script>
  Gitflow wrangles up commits and merges with a clear and formal workflow for promoting production-ready code, and separating the development of new features, while still allowing for breakfixes to be made.
</script>
***
# OK, How?
![gitflow](https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAKMAAAAJDM2NjY0OWE4LTc0NDAtNDdkMS1hMDdiLWU3MzkwM2FjYWExNw.png)
<script>
  There's a simple methodology behind this madness. Just study this image and you've got it! Just kidding, I'll walk you through how each of these branches are used.
</script>
***
# So there are Main Branches
![master](http://imgur.com/RTSMp13.png)
![develop](http://imgur.com/Ywlvoyn.png)
- Exist for the life of the project
- **Develop** contains work in progress
- **Master** contains production releases
<script>
  Develop and Master are the two main branches. The Develop branch is for the code you're working to release. The Master branch is for the code you have released. These branches will exist for the life of the project. You don't ever commit directly to these branches, but rather merge in code.
</script>
***
# ...and Developmental Branches
![Features](http://i.imgur.com/nc0lxah.png)  
![Release](http://i.imgur.com/qP1rzLS.png)  
![Hotfix](http://i.imgur.com/v2n7zz7.png)  
- Exist until merged into develop or release branches, or abandoned
- **Feature** contains developmental new features
- **Release** contains prepared releases from **Develop**
- **Hotfix** contains production fixes

<script>
  The three branches you actually do work and commit to are feature branches, release branches, and hot-fixes. Feature branches are for developing as many new features as you'd like. Feature branches promote code to the Develop branch. Release branches, are for when you're ready to promote code from the Develop branch to Master for production. Making changes on the release branch is only for minor tweaks and release notes. And then there are hot-fixes that are for squashing any bugs and making minor changes that need to go directly to production code, and - most importantly - are not a new feature. When code is promoted to Develop and/or Master, the commits are integrated into the main branches and then the developmental branch is deleted. If you decide to abandon any changes before merging them into one of the main branches you can delete that developmental branch and continue to develop without having to back out of any changes.
</script>
***
# What?  
\*<\sigh>\*  
To put it plain and simple:
<script>
  I know that can be a lot to take in so let me break it down for you.
</script>
***
# Start a feature -> Do some work -> Finish feature = Develop has new feature
![feature](http://i.imgur.com/FoZq9Md.png)
<script>
  Look at it this way. These gold circles represent commits and the grey circles represent merges. You start this flow by creating a feature, then working on it, and finally integrating that feature back into the develop branch or abandoning it.
</script>
***
# Create a release -> Tweak and write release note -> Finish Release = Production and Develop has new release
![release](http://i.imgur.com/uzkIGRu.png )
<script>
  When you're code is solid and you're ready to release it to the world, you create a release branch, add in minor changes and any release notes, and then it's merged into the develop and master branches.
<script>
***
# Create a hotfix -> Correct some code -> Finish hotfix = Production and Develop have hotfix
![hotfix](http://i.imgur.com/kD4DsMb.png)
<script>
  If there are any breakfixes that need to be made, you start a hotfix, make the code changes and then it's merged into the main branches.
</script>
***
# Repeat.
<script>
  And this beautiful cycle is repeated for eternity, or at least for the life of your project.
</script>

***
# Demo
## Gitflow_init - Source Tree
1. Initialize & First commit
2. Create Feature
## Gitflow CLI
git flow init
git flow feature start SolarSystem
git flow feature finish SolarSystem
## Gitflow_feature - Source Tree
3. Create a release
4. Demonstrate how a release is merged into master and develop
5. Create a hot-fix
<script>
  I'd like to now show you git flow in action
</script>
***
# Keep in Mind
- Developmental branches are given a respective naming convention (e.g. **feature**/Neptune, **release**/10000000, **hotfix**/pluto)
- Anything that goes to production is tagged (e.g. v1.0)
- Rebase when finishing **Feature** to keep **Develop** commit history linear and clean
- You can use [Git Hooks](https://github.com/jaspernbrouwer/git-flow-hooks) to automate naming tags/versions
<script>
  Some things you may want to remember. There is a naming convention associated with branches and as you create new features, releases and hotfixes, they will take on their respective prefix. Production-level code in master is tagged. After finishing a feature, I suggest rebasing to keep your code pristine. Once you've found your gitflow groove, you can even automate tag numbers being bumped and specifying tag messages through git hook scripts.
</script>
***
# Woah!
![glitter-toss](http://i.giphy.com/xTiTnEHBh7qapyuvwQ.gif)
*You're welcome!*
<script>
  Yea, Gitflow is pretty awesome and it makes your code play nice with others and more importantly helps you and your team release code quicker and cleaner, just how coding should be. Any questions?
</script>
***
# References
<script>
  Attached is a reference to more material on Gitflow and how to set it up on your computer for a CLI and SourceTree.
</script>
- Gitflow's Branching Model Strategy- http://nvie.com/posts/a-successful-git-branching-model/
- Gitflow SourceTree- https://blog.sourcetreeapp.com/2012/08/01/smart-branching-with-sourcetree-and-git-flow/
- Gitflow CLI- https://github.com/nvie/gitflow
- Continuous Delivery & Gitflow- https://www.atlassian.com/continuous-delivery/continuous-delivery-workflows-with-feature-branching-and-gitflow
- Git Hooks- https://github.com/jaspernbrouwer/git-flow-hooks
- Glitter Toss- http://i.giphy.com/xTiTnEHBh7qapyuvwQ.gif
