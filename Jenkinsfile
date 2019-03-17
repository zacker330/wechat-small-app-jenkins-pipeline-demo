pipeline {
  agent { label "mac"}
  environment{
    WX_CLI_CLI = '/Applications/wechatwebdevtools.app/Contents/Resources/app.nw/bin/cli'
    PREVIEW_QR_CODE_PATH = "${WORKSPACE}/qr.txt"
    WX_APP_VERSION = ''
    QR_IMAGE_PATH = "${WORKSPACE}/qr.jpg"
  }
  stages{
    stage("生成版本号"){
      agent {label "mac" }
      steps{
        script{
            WX_APP_VERSION = generateVersion("1.0.0")
      }}}
    stage("编译并执行单元测试"){
      steps{
        // sh "${WX_CLI_CLI} -t ${WORKSPACE}"
        echo "根据项目实际情况执行测试命令"
      }}
    stage("生成预览二维码"){
      steps{
        sh "${WX_CLI_CLI} -p ${WORKSPACE} --preview-qr-output base64@${PREVIEW_QR_CODE_PATH}"
        sh "cat ${PREVIEW_QR_CODE_PATH} | base64 -D > ${QR_IMAGE_PATH}"
      }
      post{
        success {
          uploadQRcodeImage("${WX_APP_VERSION}", "${QR_IMAGE_PATH}")
        }
      }
    }
    stage("手工测试"){
      steps{
        input message: "手工测试是否通过"
      }}
    stage("上传代码"){
      steps{
        sh "${WX_CLI_CLI} cli -u ${WX_APP_VERSION}@${WORKSPACE} --upload-desc 'release ${WX_APP_VERSION}'"
      }}
  }
  post {
  always { cleanWs() } } // 清理工作空间
}

def generateVersion(def ver){
  def gitCommitId = env.GIT_COMMIT.take(7)
  return "${ver}-${gitCommitId}.${env.BUILD_NUMBER}"
}

def uploadQRcodeImage(def ver, def imagePath){
  echo "将预览二维码上传到指定位置，方便测试人员扫码测试"
}