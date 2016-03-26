# Deploying Meteor Application to AWS Elastic Beanstalk

## Meteor App Preparation
1. Create node.config file in .ebextensions

        option_settings:
          - namespace: aws:elasticbeanstalk:container:nodejs
            option_name: NodeVersion
            value: 0.12.10
            
2. Put meteor app into bitbucket/github

## Codeship Setup

1. Codeship test settings

        curl -o meteor_install_script.sh https://install.meteor.com/
        chmod +x meteor_install_script.sh
        sed -i "s/type sudo >\/dev\/null 2>&1/\ false /g" meteor_install_script.sh
        ./meteor_install_script.sh
        export PATH=$PATH:~/.meteor/

2. Codeship deployment settings

    1. Custom script
    
            meteor build . --directory
            cd bundle
            cp -R ../.ebextensions .
            cp programs/server/package.json
    
    2. Elastic Beanstalk
    
            AWS Access Key ID: ****************ARCA
            AWS Secret Access Key: ************************************nWC+
            Region: us-west-2
            Application Name: sampleapp
            Environment Name: sampleapp-env
            S3 Bucket: elasticbeanstalk-us-west-2-938561245568
        
        A S3 bucket will be automatically created when you create application in EBS, put the bucket name in the settings.
        
3. Configure Elastic Beanstalk environment
    
    1. Open Configuration -> Software Configuration
    
    2. Add Environmental Variable
    
            ROOT_URL --> http://sampleapp-env.us-west-2.elasticbeanstalk.com/  

    3. Change node command to
    
            node main.js    
