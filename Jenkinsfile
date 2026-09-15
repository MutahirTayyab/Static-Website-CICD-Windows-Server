pipeline {

agent any

options {
    skipDefaultCheckout(true)
}
stages {


stage('Checkout and Update Code'){
steps{

        bat """

        if exist .git (

            echo Repository exists
            git fetch origin main
            git reset --hard origin/main

        ) else (

            echo First time checkout
            git clone https://github.com/MutahirTayyab/Static-Website-CICD-Windows-Server.git

        )

        """

}

}



stage('Deploy Website Files'){

steps{

                bat """

                echo Deploying Static Website...

                xcopy /E /I /Y C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\Static-Website-Windows-Server\\* C:\\inetpub\\wwwroot\\Static-Website-CICD-Windows-Server\\

                echo Deployment Completed

                """


}

}


}


}