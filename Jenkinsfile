pipiline {
agent any
stages {
stage ('build') { //la phase de build
steps {
sh './gradlew build'
archiveArtifacts 'build/libs/*.jar'
}
}
}
}