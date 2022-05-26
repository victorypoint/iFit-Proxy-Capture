# iFit-Proxy-Capture
iFit workout - SSL/TLS data capture

Using a SSL/TLS proxy while connected to a NordicTrack Commericial 2950 iFit Embedded Wifi treadmill (2021 model), I've collected a number of logs from a manual workout where I change speed and incline.

1. Manual workout sequence:

- Start treadmill and login to iFit
- From iFit dashboard screen, select manual workout
- Start at 2 kph and manually increment speeds from 2 kph - 13 kph
- Go through preset speeds - 1.6, 3.2, 4.8, 6.4, 8.0, 9.6, 11.3, 12.9, 14.6 - then back to 1.6 kph
- Go through preset positive inclines - 1, 2, 3, 4, 6, 8, 10, 12, 15 - then back to 0
- Go through preset negative inclines - -1, -2, -3 - then back to 0
- Stop treadmill and stop workout
- Return to Fit dashboard screen

2. iFit logs captured with BurpSuite Proxy (running on a Windows 11 PC):

- 11 iFit events were captured sequentially as I stepped through the workout sequence described above
- iFit events are captured as JSON data
- ifit.xml (not very readable)
- ifit.xml.html (readable HTML)

3. iFit workout data captured as Raygun4Net logs

- Treadmill workouts are recorded within JSON Raygun4Net data which contain Amazon AWS S3 cloud storage links to last 3 days of iFit log files
- iFit workout data is captured as Raygun4Net log files (https://github.com/MindscapeHQ/raygun4net) which are uploaded as Amazon S3 cloud storage logs by date (2022-05-13)

- The Amazon S3 cloud storage links are uploaded as gz archives
- Within the Amazon S3 log files, manual workout entries for speed and incline changes can be found, for example:

- [Trace:FitPro] Changed KPH to: 1.6
- [Trace:FitnessConsole] Kph changed from 3.21 to 1.6

- [Trace:FitPro] Changed Grade to: 1
- [Trace:FitnessConsole] Grade changed from 0 to 1

- Also within the Amazon S3 log files, entries describe where iFit local data is stored:

- [Trace:CacheFileManager] LocalMachine Cache Directory is /data/user/0/com.ifit.standalone/cache
- [Trace:CacheFileManager] Secure Cache Directory is /data/user/0/com.ifit.standalone/files/Secret

4. iFit JSON events as described in BurpSuite Proxy ifit.xml.html (readable HTML) - 10 events

{"EventData":[{"SessionId":"b917bda2-d688-475ab6c6-7350dbb3bee5","Timestamp":"2022-05-13T14:55:18.17652Z","Type":"session_start","User":{"Identifier":"3401fdfadd556c5d"
,"IsAnonymous":true,"UUID":"3401fdfadd556c5d"},"Version":"3818 / 2.6.78","OS":"Android","OSVersion":"9","Platform":"Malata 
MalataMediatekArgon2"}]}

{"EventData":[{"SessionId":"b917bda2-d688-475ab6c6-7350dbb3bee5","Timestamp":"2022-05-13T14:55:45.108394Z","Type":"mobile_event_timing","User":{"Identifier":"alanudell",
"IsAnonymous":false,"Email":"xxxx","FullName":"xxxx"},"Version":"2.6.78.3818","OS":"Android","OSVersion":"9","Platform":"Malata MalataMediatekArgon2","Data":"[{\"Name\":
\"LaunchScreenView\",\"Timing\":{\"Type\":\"p\",\"Duration\":111}}]"}]}

{"EventData":[{"SessionId":"b917bda2-d688-475ab6c6-7350dbb3bee5","Timestamp":"2022-05-13T14:55:46.503092Z","Type":"mobile_event_timing","User":{"Identifier":"xxxx",
"IsAnonymous":false,"Email":"xxxx","FullName":"xxxx"},"Version":"2.6.78.3818","OS":"Android","OSVersion":"9","Platform":"Malata MalataMediatekArgon2","Data":"[{\"Name\":
\"AccountSelectionView\",\"Timing\":{\"Type\":\"p\",\"Duration\":478}}]"}]}

{"EventData":[{"SessionId":"b917bda2-d688-475ab6c6-7350dbb3bee5","Timestamp":"2022-05-13T14:56:01.341013Z","Type":"mobile_event_timing","User":{"Identifier":"xxxx",
"IsAnonymous":false,"Email":"xxxx","FullName":"xxxx"},"Version":"2.6.78.3818","OS":"Android","OSVersion":"9","Platform":"Malata MalataMediatekArgon2","Data":"[{\"Name\":
\"DashboardView\",\"Timing\":{\"Type\":\"p\",\"Duration\":1150}}]"}]}

{"EventData":[{"SessionId":"b917bda2-d688-475ab6c6-7350dbb3bee5","Timestamp":"2022-05-13T14:56:39.159252Z","Type":"mobile_event_timing","User":{"Identifier":"xxxx",
"IsAnonymous":false,"Email":"xxxx","FullName":"xxxx"},"Version":"2.6.78.3818","OS":"Android","OSVersion":"9","Platform":"Malata MalataMediatekArgon2","Data":"[{\"Name\":
\"PreparingWorkoutView\",\"Timing\":{\"Type\":\"p\",\"Duration\":409}}]"}]}

{"EventData":[{"SessionId":"b917bda2-d688-475ab6c6-7350dbb3bee5","Timestamp":"2022-05-13T14:56:42.712543Z","Type":"mobile_event_timing","User":{"Identifier":"xxxx",
"IsAnonymous":false,"Email":"xxxx","FullName":"xxxx"},"Version":"2.6.78.3818","OS":"Android","OSVersion":"9","Platform":"Malata MalataMediatekArgon2","Data":"[{\"Name\":
\"InWorkoutView\",\"Timing\":{\"Type\":\"p\",\"Duration\":2944}}]"}]}

{"OccurredOn":"2022-05-13T14:56:48.245746Z","Details":{"MachineName":"MalataMediatekArgon2","Version":"2.6.78.3818","Err
or":{"Data":{},"ClassName":"System.NullReferenceException","Message":"Object reference not set to an instance of an 
object","StackTrace":[{"LineNumber":0,"ClassName":"Wolf.Core.Hollywood.Services.VideoBufferMonitoring.VideoBufferMonitoring
Service","FileName":"<a3dcaf78efc2480da7cb2765e39ff8cc>","MethodName":"StartVideoBufferMonitoring()","Raw":"at 
Wolf.Core.Hollywood.Services.VideoBufferMonitoring.VideoBufferMonitoringService.StartVideoBufferMonitoring () [0x000f3] in 
<a3dcaf78efc2480da7cb2765e39ff8cc>:0"}]},"Environment":{"ProcessorCount":4,"OSVersion":"28","WindowBoundsWidth":1920,"
WindowBoundsHeight":1024,"CurrentOrientation":"Rotation 0 (Portrait)","Architecture":"arm64 -
v8a","Model":"MalataMediatekArgon2 / iFit 
Embedded","TotalPhysicalMemory":10605696,"AvailablePhysicalMemory":5065184,"DeviceManufacturer":"Malata","UtcOffset": -3,
"Locale":"English"},"Client":{"Name":"Raygun4Net.Xamarin.Android","Version":"5.7.1.0","ClientUrl":"https://github.com/Mindsca p
eHQ/raygun4net"},"Tags":["Mobile","UnobservedTaskException","Crash","BuiltIn","Prod"],"UserCustomData":{"message":"System.
Threading.Tasks.Task`1[System.Threading.Tasks.VoidTaskResult]","branch":"dev","s3-logs":["https://xxxx.gz","https://xxxx.gz","https://xxxx.gz"]},"User":{"Identifier":"xxxx","IsAnonymous":false,"Email":"xxxx","FullName":"xxxx"}}}

{"EventData":[{"SessionId":"b917bda2-d688-475ab6c6-7350dbb3bee5","Timestamp":"2022-05-13T15:04:11.330749Z","Type":"mobile_event_timing","User":{"Identifier":"xxxx",
"IsAnonymous":false,"Email":"xxxx","FullName":"xxxx"},"Version":"2.6.78.3818","OS":"Android","OSVersion":"9","Platform":"Malata MalataMediatekArgon2","Data":"[{\"Name\":
\"PostWorkoutWebviewView\",\"Timing\":{\"Type\":\"p\",\"Duration\":238}}]"}]}

{"OccurredOn":"2022-05-13T15:04:16.775416Z","Details":{"MachineName":"MalataMediatekArgon2","Version":"2.6.78.3818","Err
or":{"Data":{"AlreadySentByRaygun":true},"ClassName":"Shire.Core.Util.Events.WorkoutPathoutService+WorkoutPathoutUnknown
ReasonException","Message":"Exception of type 
'Shire.Core.Util.Events.WorkoutPathoutService+WorkoutPathoutUnknownReasonException' was 
thrown.","StackTrace":[{"LineNumber":0,"ClassName":"Shire.Core.Util.Events.WorkoutPathoutService","FileName":"<1e00d4c1d0c
14743bb1075e8a94381d3>","MethodName":"FinishAsync(System.String reason, System.String subreason, 
System.Collections.Generic.Dictionary`2[TKey,TValue] properties)","Raw":"at 
Shire.Core.Util.Events.WorkoutPathoutService.FinishAsync (System.String reason, System.String subreason, 
System.Collections.Generic.Dictionary`2[TKey,TValue] properties) [0x00192] in 
<1e00d4c1d0c14743bb1075e8a94381d3>:0"}]},"Environment":{"ProcessorCount":4,"OSVersion":"28","WindowBoundsWidth":1920
,"WindowBoundsHeight":1024,"CurrentOrientation":"Rotation 0 (Portrait)","Architecture":"arm64 -
v8a","Model":"MalataMediatekArgon2 / iFit 
Embedded","TotalPhysicalMemory":14331232,"AvailablePhysicalMemory":2043648,"DeviceManufacturer":"Malata","UtcOffset": -3,
"Locale":"English"},"Client":{"Name":"Raygun4Net.Xamarin.Android","Version":"5.7.1.0","ClientUrl":"https://github.com/Mindsca p
eHQ/raygun4net"},"Tags":["Mobile","Manual","BuiltIn","Prod"],"UserCustomData":{"message":"WorkoutPathoutService : Reason: 
unknown","branch":"dev","s3-logs":["https://xxxx.gz","https://xxxx.gz","https://xxxx.gz"]},"User":{"Identifier":"xxxx","IsAnonymous":false,"Email":"xxxx","FullName":"xxxx"}}}

{"EventData":[{"SessionId":"b917bda2-d688-475ab6c6-7350dbb3bee5","Timestamp":"2022-05-13T15:07:04.754301Z","Type":"session_end","User":{"Identifier":"xxxx","IsAnony
mous":false,"Email":"xxxx","FullName":"xxxx"},"Version":"2.6.78.3818","OS":"Android","OSVersion":"9","Platform":"Malata MalataMediatekArgon2"}]}
 Fitness Page 3 
