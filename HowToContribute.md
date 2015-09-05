## Philosophy ##

The purpose of the TE is to show/teach developers how to write a more testable code. In that sense it is very important that the TE codebase itself shows how to write good testable code and focuses on tests which prove the TE works.

## Expectations ##

  * [Hudson Continuous Build](http://build.hevery.com/job/Testability-Explorer/).
  * We aim to keep the build green(blue) all of the time. So if you break it revert your change and try to fix the problem before you submit it back.
  * We aim to make sure that any green(blue) build is releasable build. Therefore never check in anything which makes the build pass but actually breaks the product.
  * All of the code is fully tested with with at least 80% test coverage. Preferably test-driven.
  * Most of our tests are of unit-test kind. (ie small tests focusing on a class or two at a time.)
  * Before you commit (or if you don't have commit rights) please have others do review on http://codereview.appspot.com/ and cc/email [testability-explorer-dev@googlegroups.com](mailto:testability-explorer-dev@googlegroups.com)

## Contribution ##
We are always eager to have people contribute new features to the project. So your feedback and work are more than welcome. We are here to help you with any questions and guidance so please ask. But we have few rules:

### As a New Contributor ###
As a new contributor we expect you to follow our philosophy and expectations above. Please send us your patch and we will incorporate it in on your behalf or give your feedback if it does not follow our guidelines.

### As a Seasoned Contributor ###
As a someone who has sent us a lot of patches and who follows our guidelines we are happy to add your to our project list owners and we trust that you will adhere to our guidelines on code quality.

### A No-No: Don't even think about it... ###
  * Global Variables / State / Singletons.
  * Code without test.
  * Code whose Testability Cost is greater then 30.

## Developing on Testability Explorer ##
  * Checkout from google code - http://code.google.com/p/testability-explorer/source/checkout
  * Install Maven - http://maven.apache.org
  * Set some environment variables, so the Eclipse plugin will build. You may want to download the RCP/PDE plugin development version of Eclipse so that the plugin tools are available. Otherwise you'll need to figure out the appropriate plugins to add to an Eclipse install. If you use IDEA, well, I feel your pain here.
    * `ECLIPSE_LAUNCHER_JAR=<eclipse dir>/plugins/org.eclipse.equinox.launcher_1.0.101.R34x_v20081125.jar`
    * `ECLIPSE_PDE_XML=<eclipse dir>/plugins/org.eclipse.pde.build_3.4.1.R34x_v20081217/scripts/build.xml`
    * `ECLIPSE_BASE_DIR=<eclipse dir>`
  * From the root folder, run `mvn install`. This should build everything, run the tests, and copy binaries to your local maven repository (~/.m2/repository)
  * To set up your IDE:
    * Eclipse: Run `mvn eclipse:eclipse` to create the .project and .classpath metadata files. Then, in Eclipse, do Import -> Existing projects into workspace, pick the root testability-explorer folder, and add all the suggested projects. You may want to add them to a working set so they are grouped together. Then, add a Classpath Variable (Preferences -> Java -> Build Path) called M2\_REPO and point it to ~/.m2/repository.
    * IDEA: Just create a new project using an external model, and point to the Maven pom.xml file in the root testability-explorer folder.