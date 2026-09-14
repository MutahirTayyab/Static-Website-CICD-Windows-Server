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
            git clone https://github.com/MutahirTayyab/Mutahir_Static_Portfolio.git .

        )

        """

}

}



stage('Deploy Website Files'){

steps{

                bat """

                echo Deploying Static Website...

                xcopy /E /I /Y "%WORKSPACE%\\*" "C:\\inetpub\\wwwroot\\Mutahir_Static_Portfolio\\"

                echo Deployment Completed

                """


}

}


}


}