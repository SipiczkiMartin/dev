/* find all variables in jenkins => /env-vars.html after jenkins server site */

def gv //define a variable

CODE_CHANGES = getGitChanges() //can be a groov script that checkes something 
                                //CODE_CHANGES is a var that we defined
pipeline {  //required
      agent any //required can specify agent instead of any
      parameters {
        string(name: 'NAME',defaultValue: '',description: '')
        choice(name: 'CHOICE',choices: ['1','2','3'],description:'')
        booleanParam(name: 'executeTests',defaultValue: true,description:'')
      }
      tools{//to be able to use tools like maven gradle or jdk in the file
        maven 'Maven' //single quotes see the installed tools in maven to know the name in quotes
      
      }
      environment { //here we define our own environmental variables
          NEW_VERSION = '1.3.0' //like this ;)
          SERVER_CREDENTIALS = credentials('credentialID') //this method is available if 
                              Credentials binding plugin is installed in Jenkins!!
      }
      stages{ //different stages of pipeline also required
        
        stage("build") {  
          
          steps { //here goes script that executes on server
              //sh or java
              echo 'building app'
              echo "building version ${NEW_VERSION}" //has to be in "" !!!
              sh "mvn install" //this is usable only after adding maven tool!!
              
              script{ //write groovy scripts here
                gv = load "scriptname.groovy)
                gv.groovyFunction() //function defined in a froovy sctipt
                                    //we can use all our defined parameters in the groovy script
                                      same as here ${params.NAME}
              }
          }
          
        }
        
        stage("test") {
        
           when{ //define when this step should run
           
              expression{ //boolean expressions
                  BRANCH_NAME == 'dev' //BRANCH_NAME is jenkins predefined var
                  //can use || && as well
                  CODE_CHANGES == true
                   
                  params.executeTests == true //we use parameters like this even in ${params.NAME}
              }
              
           }
             //now this step will only execute if the branch is dev
          steps { //here goes script that executes on server
              //sh or java
              echo 'testing app'
          }
          
        }
        
        stage("deploy") {
        
          steps { //here goes script that executes on server
              //sh or java
              echo 'deploying app'
          }
          
        }
      }
      
      post{  //steps that run after execution
      
        always{  //runs always
        
        }
        
        failure{ //runs after step failure
        
        }
      }
 }
