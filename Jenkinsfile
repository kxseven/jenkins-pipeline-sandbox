#!groovy

/*
The MIT License

Copyright (c) 2015-, CloudBees, Inc., and a number of other of contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

        THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
*/


node() {

    currentBuild.result = "SUCCESS"

    try {

       stage('Cleanup OLD Run(s)'){

            echo 'Docker cleanup'
            sh 'docker stop blue-demo-app'
            sh 'docker rm blue-demo-app'
            sh 'docker volume rm demoapp_data'
            sh 'docker volume rm demoapp_config'

       }

       stage('Checkout'){

          checkout scm

       }

       stage('Build Docker'){

        sh 'docker build --tag blue-demo-app:1'

       }

       stage('Deploy'){

        echo 'Deploy container locally'
        sh 'docker volume create demoapp_data'
        sh 'docker volume create demoapp_config'
        sh 'docker run -d -p 5000:5000 -v demoapp_data:/data -v demoapp_config:/config --name blue-demo-app blue-demo-app:1'

       }


    }
    catch (err) {

        currentBuild.result = "FAILURE"

            mail body: "project build error is here: ${env.BUILD_URL}" ,
            from: 'xxxx@yyyy.com',
            replyTo: 'yyyy@yyyy.com',
            subject: 'project build failed',
            to: 'zzzz@yyyyy.com'

        throw err
    }

}
