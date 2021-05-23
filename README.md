# AndroidCICD
A Repository to implement CICD Principals and thoery

Below is me explaining the theory in my own words, Please make pull request if you feel like my information can be improved

A best pracitce to deliver new code changes from developers

CI(continous Integration) allows devs to merge into Main Repo without conflicts

You can set to test workflow and release a apk every time push is triggered allowing teams to detect problems asap

![branch](https://user-images.githubusercontent.com/11702972/119254722-8d29cb80-bbea-11eb-9d8d-d6cfc1a20216.png)
   Above image we see that dev makes feature branch and adds new feature and its only merged to master if its bettle tested and good to go

CI uses light weight abstraction called containers(Ex:Docker)
  - Containers allows a developer to package a application with all its dependencies and deploy it as one package
  - A popular tool is docker and these containers are distributed as docker images

#Github workflows using actions
 Types
 1. Events - Specific activities that trigger workflows
 2. Jobs - set of steps run on a fresh instance of the Virtual eniviroment either sequentially or parrell
 3. Runners - execute jobs in workflows
 4. Actions - smallest portable block of a workflow
 5. Artifacts - are files likes APKs which workflow generates

#Doing CI with github

Above steps are achieved through .yaml file containing the procedures

#Define a workflow

Create a folder structure as such .github/workflow/(your yaml file)

#Running Unit test

- name: Unit tests
  run: ./gradlew testDebugUnitTest

#Running Instrumental tests

You need a emulator and a custom runner its recommended to switch off animations to avoid flaky tests
.html and .xml files generated end of test result

- name: Run instrumentation tests
        uses: reactivecircus/android-emulator-runner@v2
        with:
          api-level: 29
          arch: x86
          profile: Nexus 6
          avd-name: test
          emulator-options: -no-window -gpu swiftshader_indirect -no-snapshot -noaudio -no-boot-anim -camera-back none
          disable-animations: true
          script: ./gradlew connectedCheck
          
#Generating a APK

Since no emulators are involved this can be executed in ubuntu-latest runner. ./gradlew assembleDebug command generates an APK app-debug.apk at app/build/outputs/apk/debug/ directory. In the final step, provide this path to actions/upload-artifact@v1 action to upload it as an artifact.

#Code Coverage

Code coverage is there to fail the workflow when the output of the test hasnt touched all the code

#State Coverage

covers deifferent possible out comes of the code to achieve a succesfull CI

A function with one Bool argument can have two states: when the argument is true and when the argument is false. 
Similarly, a function with two Bool arguments will have 22 states. Now, imagine the number of states a function with an Int argument have.

#Setting up JaCoCo

Once you set up JaCoCo , unit tests generates .exec file inside the JaCoCo directory

while instrumental tests generate .ec file in outputs/code_coverage/debugAndroidTest

#Making the build fail

You need to setup a file that amkes the build fail when inadequate test cases and dont meet the criteria
