# SMKitUI iOS Demo
----------------
## 1. Installation

### Cocoapods
```
// [1] add the source to the top of your Podfile.
source 'https://bitbucket.org/sency-ios/sency_ios_sdk.git'
source 'https://github.com/CocoaPods/Specs.git'

// [2] add the pod to your target
target 'YourApp' do
  use_frameworks!
  pod 'SMKitUI'
end

// [3] add post_install hooks
post_install do |installer|
  installer.pods_project.targets.each do |target|
    target.build_configurations.each do |config|
      config.build_settings['BUILD_LIBRARY_FOR_DISTRIBUTION'] = 'YES'
      config.build_settings['EXCLUDED_ARCHS[sdk=iphonesimulator*]'] = 'arm64'
      config.build_settings['IPHONEOS_DEPLOYMENT_TARGET'] = '14.5'
    end
  end
end

```

### SPM

Comming soon

## 2. Setup

Add camera permission request to `Info.plist`
```
<key>NSCameraUsageDescription</key>
<string>Camera access is needed</string>
```

## 3. Configure
```
SMKitUIModel.configure(authKey: "YOUR_KEY") {
    // The configuration was successful
    // Your Code
} onFailure: { error in
    // The configuration failed with error
    // Your Code
}
```
To reduce wait time we recommend to call `configure` on app launch.

**⚠️ SMKitUI will not work if you don't first call configure.**
 
## 4. Start
Implement **SMKitUIWorkoutDelegate**.
```
extension ViewController:SMKitUIWorkoutDelegate{
    // Runtime error callback
    func handleWorkoutErrors(error: Error) {
        
    }

    // Workout session end callback
    func workoutDidFinish(summary: WorkoutSummaryData) {
        // Will close SMKitUI.
        SMKitUIModel.exitSDK()
    }

    // Exit workout callback
    func didExitWorkout(summary: SMKitUIDev.WorkoutSummaryData) {
        //Will close SMKitUI.
        SMKitUIModel.exitSDK()
    }
}
```
    
- Assessmet:  
**startAssessmet** starts one of Sency's blueprint assessments.
```
func startAssessmentWasPressed(){
    do{
        // Start assessment
        try SMKitUIModel.startAssessmet(viewController: self, type: AssessmentTypes.Fitness, delegate: self)
    }catch{
        showAlert(title: error.localizedDescription)
    }
}
```

- Custom Workout:
**startWorkout** starts a custom workout.
```
// List of exercises
let exercise:[SMExercise] = [
    SMExercise(
        name: "The name of the exercise",
        exerciseIntro: "(optinal) a path to intro sound",
        totalSeconds: 30, // exercise time
        introSeconds: 10, // (optinal) time before the start of the exercise when the big video instruction will be shown
        videoInstruction: "(optinal) a path to a video instruction",
        uiElements: [.gaugeOfMotion,.timer,.repsCounter], // the UI elements you will like to be shown
        detector: "The exercise you will like",
        repBased: true, // is the exerise repetition based
        exerciseClosure: "(optinal) a path to outro sound"
    )
]

let workout = SMWorkout(
    id: "The workout id",
    name: "The workout name",
    workoutIntro: "(optinal) a path to intro sound",
    soundtrack: "(optinal) a path to a soundtrack",
    exercises: exercise, // The exercises we add Before
    workoutClosure: "(optinal) a path to outro sound"
)

do{
    try SMKitUIModel.startWorkout(viewController: self,workout: workout, delegate: self)
}catch{
    print(error)
}
```

Having issues? [Contact us](support@sency.ai) and let us know what the problem is.
