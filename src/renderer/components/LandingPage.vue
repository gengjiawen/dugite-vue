<template>
  <div id="wrapper">
    <button @click="download">Click me to test dugite, check output in Console</button>
  </div>
</template>

<script>
  import SystemInformation from './LandingPage/SystemInformation'
  import {GitProcess} from 'dugite'
  import os from 'os'
  import fs from 'fs-extra'

  export default {
    name: 'landing-page',
    components: { SystemInformation },
    methods: {
      download () {
        const desDir = os.tmpdir() + Date.now()
        console.log(desDir)
        fs.ensureDirSync(desDir)
        const args = ['clone', '--', 'https://github.com/desktop/dugite', desDir]
        GitProcess.exec(args, __dirname)
          .then(result => {
            console.log('response')
            if (result.exitCode === 0) {
              console.log('congratulations')
              const output = result.stdout
              console.log(output)
            } else {
              const error = result.stderr
              console.log(error)
            }
          })
          .catch(e => {
            console.log('error')
            console.log(e)
          })
      }
    }
  }
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=Source+Sans+Pro');

  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body { font-family: 'Source Sans Pro', sans-serif; }
</style>
