# video-streaming-alexa-skill
Skill for streaming video in alexa devices with screen (e.g: echo show)

## SKILL COMMANDS

1. Request a particular video: "Alexa, ask Echotube Player to play 'Charley bit my finger'"
2. Request an auto generated playlist of 25 results: - "Alexa ask Echotube Player to play"
3. Request a particular track from the playlist: "Alexa, ask Echotube Player to play Track 10"
4. Skip to the next/previous track:- "Alexa, next/ previous track"
5. Pause:- "Alexa pause" or "Alexa stop"
6. Resume playback:- "Alexa resume" 
7. Find out what is playing by asking "Alexa ask Echotube Player what's playing" - this will also tell you your data usage
8. Turn Autoplay of the next video on/off:- "Alexa turn autoplay on/off" 
8. Loop the current playlist:- "Alexa Loop On/Off"
9. Shuffle mode On/Off:- "Alexa shuffle On/Off"
10. Start the track currently playing fromt he beginning:- "Alexa Start Over"
11. Get a list of these commands in the Alexa app: - "Alexa ask Echotube Player for help"
12. Increase the data limit (this will allow the skill to incur data charges from AWS):- "Alexa, ask Echotube Player to increase the data limit"
13. Reset the data limit to default of 1000MB:- "Alexa, ask Echotube Player to reset the data limit"

![alt text](screenshots/skill_card.jpeg)

## SETUP INSTRUCTIONS

## Get a Dropbox Token

1. Create a dropbox access token using the instructions here:-
https://blogs.dropbox.com/developers/2014/05/generate-an-access-token-for-your-own-account/

## Download code from github

1. Click on the green "Clone or download" button just under the yellow bar
2. Click download ZIP
3. Unzip the file 

## AWS Lambda Setup

1. Go to http://aws.amazon.com/. You will need to set-up an AWS account (the basic one will do fine) if you don't have one already. Make sure you use the same Amazon account that your Echo device is registered to. **Note - you will need a credit or debit card to set up an AWS account - there is no way around this. Please see the AWS Charges section above**

2.  Go to the drop down "Location" menu at the top right and ensure you select US-East (N. Virginia) if you are based in the US or EU(Ireland) if you are based in the UK or Germany. This is important as only these two regions support Alexa. NOTE: the choice of either US or EU is important as it will affect the results that you get. The EU node will provide answers in metric and will be much more UK focused, whilst the US node will be imperial and more US focused.


1. Select Lambda from the AWS Services menu at the top left
2. Click on the "Create a Lambda Function" or "Get Started Now" button.
3. Select "Blank Function" - this will automatically take you to the "Configure triggers" page.
4. Click the dotted box and select "Alexa Skills Kit" (NOTE - if you do not see Alexa Skill Kit as an option then you are in the wrong AWS region). 
5. Click Next 
5. Name the Lambda Function :-

    ```
    alexaVideoPlayer
    ```
    
5. Set a decription, e.g:-

    ```
    alexaVideoPlayer
    ```
    
6. Select the default runtime which is currently "node.js 6.10".
7. Select Code entry type as "Upload a .ZIP file". 

![alt text](screenshots/lambda_1.jpeg)

7. Click on the "Upload" button. Go to the folder where you unzipped the files you downloaded from Github, select index.zip and click open. 
8. Enter the following into the Environment Variables Section (If you are pasting in the Token then make sure you have no extra spaces: -

|Key           | Value|
|--------------| -----|
|DROPBOX_TOKEN|(Put the Dropbox token in here)|

![alt text](screenshots/environment_variables.jpeg) 

9. Keep the Handler as "index.handler" (this refers to the main js file in the zip).
10. Under Role - select "Create a custom role". This will automatically open a new browser tab or window.

![alt text](screenshots/new_role.jpeg)

11. Switch to this new tab or window. 
11. Under IAM Role select "Create a new IAM Role"
11. Then press the blue "Allow" box at the bottom right hand corner. The tab/window will automatically close.
11. You should now be back on the Lambda Management page. The Role box will have automatically changed to "Choose an existing role" and Role we just created will be selected under the "Existing role" box.

![alt text](screenshots/existing_role.jpeg)

12. Under Advanced Settings set Memory (MB) to 384 and change the Timeout to 1 minute 0 seconds

![alt text](screenshots/advanced_settings.jpeg)

13. Click on the blue "Next" at the bottom of the page and review the settings then click "Create Function". This will upload the index.zip file to Lambda.

![alt text](screenshots/review_function.jpeg)

14. Copy the ARN from the top right to be used later in the Alexa Skill Setup (it's the text after ARN - it won't be in bold and will look a bit like this arn:aws:lambda:eu-west-1:XXXXXXX:function:youtube). Hint - Paste it into notepad or similar.

## Alexa Skill Setup

1. In a new browser tab/window go to the Alexa Console (https://developer.amazon.com/edw/home.html and select Alexa on the top menu)
1. If you have not registered as an Amazon Developer then you will need to do so. Fill in your details and ensure you answer "NO" for "Do you plan to monetize apps by charging for apps or selling in-app items" and "Do you plan to monetize apps by displaying ads from the Amazon Mobile Ad Network or Mobile Associates?"

![alt text](screenshots/payment.jpeg)

1. Once you are logged into your account go to to the Alexa tab at the top of the page.
2. Click on the yellow "Get Started" button under Alexa Skills Kit.

![alt text](screenshots/getting_started.jpeg)

3. Click the "Add a New Skill" yellow box towards the top right.

![alt text](screenshots/add_new_skill.jpeg)

4. You will now be on the "Skill Information" page.
5. Set "Custom Interaction Model" as the Skill type
6. Select the language as English (US), English (UK) - **Note German is not currently supported**
6. Set the "Name" to 

    ```
    Video Player Skill for Alexa
    ```
    
8. Set the "Invocation Name" to 

    ```
    Echotube Player
    ```
8. Set the "Audio Player" setting to "Yes"
8. Leave the other settings in Global Fields set to "No'
9. Click "Save" and then click "Next".

![alt text](screenshots/skill_information.jpeg)

10. You will now be on the "Invocation Model" page.
11. Copy the text below into the "Intent Schema" box - Ignore the "Built-in intents for playback control box above"

    ```
    {
      "intents": [
        {
          "intent": "AMAZON.ResumeIntent"
        },
        {
          "intent": "AMAZON.PauseIntent"
        },
        {
          "intent": "AMAZON.StopIntent"
        },
        {
          "intent": "AMAZON.NextIntent"
        },
        {
          "intent": "AMAZON.CancelIntent"
        },
        {
          "intent": "AMAZON.LoopOffIntent"
        },
        {
          "intent": "AMAZON.LoopOnIntent"
        },
        {
          "intent": "AMAZON.PreviousIntent"
        },
        {
          "intent": "AMAZON.RepeatIntent"
        },
        {
          "intent": "AMAZON.ShuffleOffIntent"
        },
        {
          "intent": "AMAZON.ShuffleOnIntent"
        },
        {
          "intent": "AMAZON.StartOverIntent"
        },
        {
          "intent": "AMAZON.HelpIntent"
        },
        {
          "slots": [
            {
              "name": "search",
              "type": "SEARCH"
            }
          ],
          "intent": "SearchIntent"
        },
        {
          "slots": [
            {
              "name": "number",
              "type": "AMAZON.NUMBER"
            }
          ],
          "intent": "NumberIntent"
        },
        {
          "intent": "NowPlayingIntent"
        },
        {
          "intent": "AutoOn"
        },
        {
          "intent": "AutoOff"
        },
        {
          "intent": "DestructCode"
        },
        {
          "intent": "RaiseLimit"
        },
        {
          "intent": "ResetLimit"
        }
      ]
    }
    ```
![alt text](screenshots/intent_schema.jpeg)

12. Under Custom Slot Types:-
13. Type into the "Enter Type" field (NOTE - this is captialised) :-
    ```
    SEARCH
    ```
    
14. Copy the text below and paste into the "Enter Values" box and then click "Add" (NOTE you must click ADD)

    ```
    prince
    the fray
    the rolling stones
    toad the wet sproket
    KC and the sunshine band
    john travolta and olivia newton john
    DJ jazzy jeff and the fresh prince
    lola
    hello dolly
    love me tender
    fools gold
    roberta flack killing me softly with his song
    stevie wonder superstition
    boston
    full circle
    dubstar
    underworld
    orbital
    let me be your fantasy
    pop will eat itself
    ultra nate
    4 hours Peaceful and Relaxing Instrumental Music
    ```
![alt text](screenshots/slot_types.jpeg)
Credit to https://github.com/rgraciano/echo-sonos/blob/master/echo/custom_slots/NAMES.slot.txt for some of these

15. Copy the text below and paste them into the Sample Utterances box.

    ```
    SearchIntent play {search}
    SearchIntent find {search}
    SearchIntent play some {search}
    SearchIntent play me some {search}
    SearchIntent videos by {search}
    SearchIntent for videos by {search}
    SearchIntent for music by {search}
    NumberIntent {number}
    NumberIntent play number {number}
    NumberIntent play track {number}
    NumberIntent play track number {number}
    NowPlayingIntent what's playing
    NowPlayingIntent what song is this
    NowPlayingIntent what is this
    NowPlayingIntent what song is playing
    NowPlayingIntent what's this song
    AutoOn turn autoplay on
    AutoOn autoplay on
    AutoOff turn autoplay off
    AutoOff autoplay off
    DestructCode zero zero zero destruct zero
    RaiseLimit increase the data limit
    RaiseLimit raise the data limit
    RaiseLimit raise the limit
    ResetLimit reset the data limit
    ResetLimit reset the limit
    ```
![alt text](screenshots/utterances.jpeg) 

16. Click "Save" and then "Next".
17. You will now be on the "Configuration" page.
18. Select "AWS Lambda ARN (Amazon Resource Name)" for the skill Endpoint Type.
19. Then pick the most appropriate geographical region (either US or EU as appropriate) and paste into the box (highlighted in red in the screenshot) the ARN you copied earlier from the AWS Lambda setup.
20. Select "No" for Account Linking and leave everything under permissions unchecked
14. Click "Save" and then "Next".
![alt text](screenshots/endpoint.jpeg) 
15. There is no need to go any further through the process i.e. submitting for certification. Note - tsting the skill using the service simulator can produce unexpected results so is not recommended

## AWS CHARGES

This skill has to transfer **a lot** of data around which Amazon charges for. The first 1 Gigabyte of data transfer per month is free after which Amazon will charge $0.09 per GB.
The skill will  **try** to prevent you from going over 1000MB in a calender month (this is not a full 1GB but leaves some additonal headroom for for other skills) and will stop playing any further audio unless you specifcally ask the skill to increase the data limit by saying:-

"Alexa, ask youtube to increase the data limit"

You will then need to give the authorisation code which is:-

    ```
    ZERO ZERO ZERO DESTRUCT ZERO
    ```

This will then add an additonal 1000MB to the data limit. You can repeat this process to increase the data limit by 1000MB increments.

The Now Playing Card in the Alexa app will tell you the data that you have used to date for the current month along with an estimated cost in US dollars if you have exceed the free usage.


** NOTE - The app can only track it's own data usage. If you run other skills on AWS then these will also contribute towards usage limits. You can cheack your usage at any time by visiting:- **

https://console.aws.amazon.com/billing/home?#/


The data limit can be reset to it's default at any time with the command:-

"Alexa, ask youtube to reset the data limit"

This will prevent any further charges if you are already over the default limit


You can increase the default data limit and AWS cost rates by setting the following environment variables:-

|Key           |Description            |Possible Values| Default Value (if variable is not set)|
|--------------| ----------------------|---------------|----------------------------|
|CHARGE_PER_GIG|This is the AWS EC2 charge per GB for the first 10 TB / month data transfer out beyond the global free tier|Cost in dollars per GB|0.090| 
|MAX_DATA|The size of the data limit in bytes|size in bytes|1048576000|
 
 ### Credit where credit is due:
 
 - Readme and code pieces from: https://github.com/Fenton-Fenton-Fenton/alexa-tube
 - Amazon Alexa Skill Kit: https://developer.amazon.com/alexa-skills-kit 
