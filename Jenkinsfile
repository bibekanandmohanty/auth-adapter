node('R10k') {
  try {
    stage ('checkout scm') {
      checkout scm
    }
    stage ('clean up rpms') {
      sh "rm -f *.rpm"
    }
    stage ('create rpm') {
      echo 'mobi-ssc-paytv-auth-adapter-config  rpm'  
      sh "fpm -s dir -t rpm -a noarch -d 'mobi-ssc-paytv-auth-adapter >= 1.0.0' -n mobi-ssc-paytv-auth-adapter-config-${env.BRANCH_NAME} --prefix /opt/mobi-ssc-paytv-auth-adapter/config -v 1.1 --iteration `git rev-list HEAD --count` --rpm-auto-add-directories --rpm-user rtv --rpm-group rtv --log debug --directories /opt/mobi-ssc-paytv-auth-adapter/config partner-config ip-range-config whitelist akstream.jks"
    }

    stage ('publish rpm to pulp') {
      echo 'Publishing rpm to pulp'
      sh "pulp-admin login -u admin -p admin"
      echo 'Uploading RPM'
      sh "pulp-admin rpm repo uploads rpm --repo-id mobi-configs -f mobi-ssc-paytv-auth-adapter-config-${env.BRANCH_NAME}-1.1-`git rev-list HEAD --count`.noarch.rpm"
      echo 'publishing rpm'
      sh "pulp-admin rpm repo publish run --repo-id mobi-configs"
      echo 'logging out'
      sh "pulp-admin logout" 
    }

  }
  catch(err) {
    echo "One of the pipeline failed with the following error ${err}"
    
    
  }
}
