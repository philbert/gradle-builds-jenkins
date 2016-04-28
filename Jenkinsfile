#!groovy
stage "compile and unit test"
node {
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
parallel (
  "stream 1": { 
    node { 
      unstash "binary"
      sh "sleep 20s" 
      sh "echo hstream1"
    } 
  },
  stream2: { 
    node { 
      unstash "binary"
      sh "echo hello2"
    } 
  }
)
             
stage "assemble"
node ('whipping_boy') {
  assemble: { sh "echo 'assembing on whipping boy'" }
}
