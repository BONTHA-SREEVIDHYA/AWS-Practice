
Step by step AWS CodePipeline

Created code for python app , requirements.txt & dockerfile
Pushed the same into github 

Created codebuild first to build & package the application
<img width="1465" height="473" alt="image" src="https://github.com/user-attachments/assets/09b01e34-c8a1-4370-b540-4d8417340028" />
Step 1: Source : github authenticated with app
<img width="996" height="768" alt="image" src="https://github.com/user-attachments/assets/b7ac139f-ea23-43cf-b414-836a36d7cbd5" />
Step2: Environment : defaults
only checked in elevated priveleges for docker build 
<img width="861" height="168" alt="image" src="https://github.com/user-attachments/assets/592eff11-4285-47ac-acaf-c6118900ed89" />

Step3: Added build steps for building the image & pushing into my dockerhub
          version: 0.2
          
          env:
            parameter-store:
              DOCKER_USERNAME: username
              DOCKER_REGISTRY_PASSWORD: password
              DOCKER_REGISTRY_URL: registry
          
          phases:
            install:
              runtime-versions:
                python: 3.11
            pre_build:
              commands:
                - echo "Installing dependencies"
                - pip install -r awscloudpipeline/requirements.txt
            build:
              commands:
                - echo "Staring Build..."
                - cd awscloudpipeline
                - echo "building docker image"
                - echo "$DOCKER_REGISTRY_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin "$DOCKER_REGISTRY_URL"
                - docker build -t "$DOCKER_REGISTRY_URL/$DOCKER_USERNAME/flaskapp:latest" .
                - docker push "$DOCKER_REGISTRY_URL/$DOCKER_USERNAME/flaskapp:latest"
            post_build:
              commands:
                - echo "build Successful"

Step4: Store the environment variables in the parameter store 
<img width="1785" height="751" alt="image" src="https://github.com/user-attachments/assets/405c5d1c-b76a-497a-a5a9-c15a5d2226c9" />

Step5: For the service role created add policy to access SSM ( parameter store _ --SSM Full access 

Step6: Dry run & check 


Codepipeline:

Add source -- github repo 
Add codebuild -- your codebuild name 

          
          

