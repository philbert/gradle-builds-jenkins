#!groovy
stage "compile and unit test"
node ('master') {
  parallel (
    phase1: { sh "echo startp1; echo endphase1" },
    phase2: { sh "echo startp2; echo endphase2" },
    phase3: { sh "echo startp3; sleep 30; echo endphase3" },
    phase4: { sh "echo startp4; echo foobar; sleep 60; echo endphase4" }
  )
  sh "echo 42 > data"
  stash includes: '*', name: 'binary'
}

stage "package"
parallel  (
  stream1: { 
    node ('master') { 
      unstash "binary"
      sh "sleep 20s" 
      sh "echo stream1"
    } 
  },
  stream2: ('master') { 
    node { 
      unstash "binary"
      sh "echo hello2"
    } 
  }
)
             
stage "assemble"
node ('whipping_boy') {
  assemble: { sh "echo 'assembing on whipping boy'" }
  input message: "Does http://88.80.174.222:8080/ look ok?"
  try {
      checkpoint('Before deploy')
  } catch (NoSuchMethodError _) {
      echo 'Are we running jenkins 2?'
  }
}

stage "deploy"
node ('whipping_boy') {
  sweet: { sh "sleep 15" }
  dude: { sh "echo 'we should deploy some application somewhere'" }
}
