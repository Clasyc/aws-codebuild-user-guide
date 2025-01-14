# Create a Notification Rule<a name="notification-rule-create"></a>

You can use notification rules to notify users when important changes, such as build successes and failures, occur\. Notification rules specify both the events and the Amazon SNS topic that is used to send notifications\. For more information, see [What are notifications?](https://docs.aws.amazon.com/codestar-notifications/latest/userguide/welcome.html)

You can use the console or the AWS CLI to create notification rules for AWS CodeBuild\. <a name="notification-rule-create-console"></a>

# To create a notification rule \(console\)<a name="notification-rule-create-console"></a>

1. Sign in to the AWS Management Console and open the CodeBuild console at [https://console\.aws\.amazon\.com/codebuild/](https://console.aws.amazon.com/codebuild/)\.

1. Choose **Build**, choose **Build projects**, and then choose a build project where you want to add notifications\.

1. On the build project page, choose **Notify**, and then choose **Create notification rule**\. You can also go to the **Settings** page for the build project and choose **Create notification rule**\.

1. In **Notification name**, enter a name for the rule\.

1. In **Detail type**, choose **Basic** if you want only the information provided to Amazon EventBridge included in the notification\. Choose **Full** if you want to include information provided to Amazon EventBridge and information that might be supplied by the CodeBuild or the notification manager\.

   For more information, see [Understanding Notification Contents and Security](https://docs.aws.amazon.com/codestar-notifications/latest/userguide/security.html#security-notifications)\.

1.  In **Events that trigger notifications**, select the events for which you want to send notifications\.     
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codebuild/latest/userguide/notification-rule-create.html)

1. In **Targets**, choose **Create SNS topic**\.
**Note**  
When you create the topic, the policy that allows CodeBuild to publish events to the topic is applied for you\. Using a topic created specifically for CodeBuild notifications also helps ensure that you only add users to the subscription list for that topic that you want to see notifications about this build project\.

   After the **codestar\-notifications\-** prefix,enter a name for the topic, and then choose **Submit**\.
**Note**  
If you want to use an existing Amazon SNS topic instead of creating a new one, in **Targets**, choose its ARN\. Make sure the topic has the appropriate access policy and that the subscriber list contains only those users who are allowed to see information about the build project\. For more information, see [Configure Existing Amazon SNS Topics for Notifications](https://docs.aws.amazon.com/codestar-notifications/latest/userguide/set-up-sns.html) and [Understanding Notification Contents and Security](https://docs.aws.amazon.com/codestar-notifications/latest/userguide/security.html#security-notifications)\. 

1. To finish creating the rule, choose **Submit**\.

1. You must subscribe users to the Amazon SNS topic for the rule before they can receive notifications\. For more information, see [Subscribe Users to Amazon SNS Topics That Are Targets](https://docs.aws.amazon.com/codestar-notifications/latest/userguide/subscribe-users-sns.html)\. You can also set up integration between notifications and AWS Chatbot to send notifications to Amazon Chime chatrooms or Slack channels\. For more information, see [Configure Integration Between Notifications and AWS Chatbot](https://docs.aws.amazon.com/codestar-notifications/latest/userguide/notifications-chatbot.html)\.<a name="notification-rule-create-cli"></a>

# To create a notification rule \(AWS CLI\)<a name="notification-rule-create-cli"></a>

1. At a terminal or command prompt, run the create\-notification rule command to generate the JSON skeleton:

   ```
   aws codestar-notifications create-notification-rule --generate-cli-skeleton > rule.json
   ```

   You can name the file anything you want\. In this example, the file is named *rule\.json*\.

1. Open the JSON file in a plain\-text editor and edit it to include the resource, event types, and Amazon SNS target you want for the rule\. The following example shows a notification rule named **MyNotificationRule** for a build project named *MyBuildProject* in an AWS acccount with the ID *123456789012*\. Notifications are sent with the full detail type to an Amazon SNS topic named *codestar\-notifications\-MyNotificationTopic* when builds are successful:

   ```
   {
       "Name": "MyNotificationRule",
       "EventTypeIds": [
           "codebuild-project-build-state-succeeded"
       ],
       "Resource": "arn:aws:codebuild:us-east-2:123456789012:MyBuildProject",
       "Targets": [
           {
               "TargetType": "SNS",
               "TargetAddress": "arn:aws:sns:us-east-2:123456789012:codestar-notifications-MyNotificationTopic"
           }
       ],
       "Status": "ENABLED",
       "DetailType": "FULL"
   }
   ```

   Save the file\.

1. Using the file you just edited, at the terminal or command line, run the create\-notification\-rule command again to create the notification rule:

   ```
   aws codestar-notifications create-notification-rule --cli-input-json  file://rule.json
   ```

1. If successful, the command returns the ARN of the notification rule, similar to the following:

   ```
   {
       "Arn": "arn:aws:codestar-notifications:us-east-1:123456789012:notificationrule/dc82df7a-EXAMPLE"
   }
   ```
