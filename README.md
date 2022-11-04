# lambda-protect
Automatic GitHub webhook that adds branch protection and notification of a new branch using Lambda and API Gateway

![undefined](https://user-images.githubusercontent.com/22108519/178094290-4b100767-7bf4-492b-993b-9a2863185c06.png)

GitHub Repo is created > Sends a payload to the API gateway, API gateway transmit's the payload to the lambda function, which inturn return's a REST api call to set branch protections and mention a username that the needful has been done. 

Built off the original https://github.com/zkoppert/Auto-branch-protect and the great work done by https://github.com/learnazcloud/Auto-branch-protect I decided to clean it up and make it serverless, using just two services, lambda and api gateway. This is a low cost, easy to utilize webhook to manage your organizations GitHub security needs. 


First you will need an AWS account, and a GitHub account with an Organization

Lets start on the GitHub side by retreiveing a GitHub personal Token (this will be our key into the enviornment). 

![GHToken](https://user-images.githubusercontent.com/22108519/178059631-f0a29cdf-c463-4d61-af65-e5299c7d1dc6.PNG)

Record this key as we will need it later on!

Next lets go into AWS and start building our enviornment.

First you will want to go into AWS and move to the Lambda services page and create a Layer (we will use this later also)

![AddLayer](https://user-images.githubusercontent.com/22108519/178059774-8eef40d7-4876-40c4-8da5-2a12dfb4e89b.PNG)

Next lets Create our Function -

![CreateFunction](https://user-images.githubusercontent.com/22108519/178059802-8ec1ed94-fa0c-4d39-a2f1-26210c653968.png)

Lets set its name and runtime version of 3.8

![CreateFunction2](https://user-images.githubusercontent.com/22108519/178059842-0cfbe5d6-97b9-4517-81d1-85d60449de14.PNG)

Now that we have our basic function created we get to add the good bits. Lets add our code (lambda_function.py) 

![AddCode](https://user-images.githubusercontent.com/22108519/178059920-5f45e948-9a73-4387-99d8-41d77cc5768d.png)

Be sure to change the username to you or multiple users.

Now we can configure the code, we must get into the ENV variables for the function and add our token.

![ConfigureENV](https://user-images.githubusercontent.com/22108519/178060024-cb941f38-83a2-4ef4-bfc8-59b17da1e13b.PNG)

Next lets Add the Layer we created earlier on the function page (you'll see near the bottom where you can add a layer)

And finally at AWS we want to create our API gateway, do this by clicking "add trigger" and use the API gateway trigger. Use the following settings. 

![APIGateway](https://user-images.githubusercontent.com/22108519/178060185-ad42acb3-21e3-4e9d-bf4e-061fe278b31d.PNG)

Once added save the API Gateway frontend URL, we will need this when we move over to GitHub

Now that we have the AWS configured lets configure GitHub.

Move to the organization in which you want to deploy this, and add the webhook - Be sure to select ALL of the individual events you want this hook responsible for (this depends on what settings you are using, to view more please review https://docs.github.com/en/rest be sure to add your API gateway URL to the Payload URL.

![GHAddHookOrg](https://user-images.githubusercontent.com/22108519/178060386-1f5722a9-6c58-44a4-a12d-4338d8946c05.PNG)

Now create a new repo! Check your logs and confirm new branch protection was created and an @mention was left in your issues area. 

All set! And using Lambda means it only costs when it runs and is a very minimal cost. 


