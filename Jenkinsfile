#!groovy
@Library('roboshop-shared-library')


def configMap = [
    application: "nodejsVM"
    component: "catalogue"
]
if( ! env.BRANCH_NAME.equalsIgnoreCase('main')){
    pipelineDecission.decidePipeline(configMap)
}
else{
    echo "This is Production branch please inform CR team"
}