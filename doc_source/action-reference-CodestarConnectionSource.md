# CodestarConnectionSource<a name="action-reference-CodestarConnectionSource"></a>


|  | 
| --- |
| The connections feature is in preview release for AWS CodePipeline and is subject to change\. | 

Triggers a pipeline when a new commit is made on a third\-party source code repository\. The source action retrieves code changes when a pipeline is manually executed or when a webhook event is sent from the source provider\. Currently, Bitbucket Cloud is the only connection type supported by the `CodestarConnectionSource` action\.

After a code change is detected, you have the following options for passing the code to subsequent actions:
+ Like other existing CodePipeline source actions, `CodestarConnectionSource` can output a ZIP file with a shallow copy of your commit\.
+ `CodestarConnectionSource` can also be configured to output a URL reference to the repo for subsequent actions\.

  Currently, the git URL reference can only be used by downstream CodeBuild actions to clone the repo and associated Git metadata\. Attempting to pass a Git URL reference to non\-CodeBuild actions results in an error\.

CodePipeline prompts you to add an Bitbucket Cloud app to your repo when you create a connection\. If you use the console to create or edit your pipeline, CodePipeline creates a Bitbucket webhook that starts your pipeline when a change occurs in the repository\. You must have already created your Bitbucket account and repository before you can connect through the `CodestarConnectionSource` action\. Use your Bitbucket account when you create a connection so that CodePipeline can use the Bitbucket repository for source stages in pipelines\.

For more infomation, see [how Bitbucket apps work](https://confluence.atlassian.com/bitbucket/bitbucket-cloud-add-ons-780871938.html) in the Bitbucket developer documentation 

**Topics**
+ [Action Type](#action-reference-CodestarConnectionSource-type)
+ [Configuration Parameters](#action-reference-CodestarConnectionSource-config)
+ [Input Artifacts](#action-reference-CodestarConnectionSource-input)
+ [Output Artifacts](#action-reference-CodestarConnectionSource-output)
+ [Action Declaration \(Bitbucket Example\)](#action-reference-CodestarConnectionSource-example)
+ [Installing the CodeStar App on Bitbucket and Creating a Connection](#action-reference-CodestarConnectionSource-auth)
+ [See Also](#action-reference-CodestarConnectionSource-links)

## Action Type<a name="action-reference-CodestarConnectionSource-type"></a>
+ Category: `Source`
+ Owner: `AWS`
+ Provider: `CodeStarSourceConnection`
+ Version: `1`

## Configuration Parameters<a name="action-reference-CodestarConnectionSource-config"></a>

****ConnectionArn****  
Required: Yes  
The connection ARN that is configured and authenticated for the source provider\.

****FullRepositoryId****  
Required: Yes  
The owner and name of the repository where source changes are to be detected\.  
Example: `some-user/my-repo`

****BranchName****  
Required: Yes  
The name of the branch where source changes are to be detected\.

****OutputArtifactFormat****  
Required: No  
Specifies the output artifact format\. Can be either `CODEBUILD_CLONE_REF` or `CODE_ZIP`\. If unspecified, the default is `CODE_ZIP`\.  
The `CODEBUILD_CLONE_REF` option can only be used by CodeBuild downstream actions\.

## Input Artifacts<a name="action-reference-CodestarConnectionSource-input"></a>
+ **Number of Artifacts:** `0`
+ **Description:** Input artifacts do not apply for this action type\.

## Output Artifacts<a name="action-reference-CodestarConnectionSource-output"></a>
+ **Number of Artifacts:** `1` 
+ **Description:** The artifacts generated from the repository are the output artifacts for the `CodestarConnectionSource` action\. The source code commit ID is displayed in CodePipeline as the source revision for the triggered pipeline execution\. You can configure the output artifact of this action in:
  + A ZIP file that contains the contents of the configured repository and branch at the commit specified as the source revision for the pipeline execution\.
  + A JSON file that contains a URL reference to the repository so that downstream actions can perform Git commands directly\.
**Important**  
This option can only be used by CodeBuild downstream actions\.

## Action Declaration \(Bitbucket Example\)<a name="action-reference-CodestarConnectionSource-example"></a>

------
#### [ YAML ]

```
name: Source
actions:
  - inputArtifacts: []
    actionTypeId:
      version: '1'
      owner: AWS
      category: Source
      provider: CodeStarSourceConnection
    outputArtifacts:
      - name: SourceArtifact
    runOrder: 1
    configuration:
      ConnectionArn: "arn:aws:codestar-connections:region:account-id:connection/connection-id"
      FullRepositoryId: "some-user/my-repo"
      BranchName: "master"
      OutputArtifactFormat: "CODE_ZIP"
    name: ApplicationSource
```

------
#### [ JSON ]

```
{
    "name": "Source",
    "actions": [
        {
            "inputArtifacts": [],
            "actionTypeId": {
                "version": "1",
                "owner": "AWS",
                "category": "Source",
                "provider": "CodeStarSourceConnection"
            },
            "outputArtifacts": [
                {
                    "name": "SourceArtifact"
                }
            ],
            "runOrder": 1,
            "configuration": {
                "ConnectionArn": "arn:aws:codestar-connections:region:account-id:connection/connection-id",
                "FullRepositoryId": "some-user/my-repo",
                "BranchName": "master",
                "OutputArtifactFormat": "CODE_ZIP"
            },
            "name": "ApplicationSource"
        }
    ]
},
```

------

## Installing the CodeStar App on Bitbucket and Creating a Connection<a name="action-reference-CodestarConnectionSource-auth"></a>

The first time you use the console to add a new connection to a Bitbucket repository, you must authorize CodePipeline access to your repositories\. You choose or create an installation app that helps you connect to the account where you have created your third\-party code repository\.

 When you use the AWS CLI or an AWS CloudFormation template, you must provide the connection ARN of a Bitbucket connection that has already gone through the installation handshake\. Otherwise, the pipeline is not triggered\.

## See Also<a name="action-reference-CodestarConnectionSource-links"></a>

The following related resources can help you as you work with this action\.
+ [AWS CodeStar API Reference](https://docs.aws.amazon.com/codestar-connections/latest/APIReference/Welcome.html) – The AWS CodeStar Connections API Reference provides reference information for the available connections actions\.