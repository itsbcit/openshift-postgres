desc "Update Dockerfile templates"
task :default do

  variants = [
    'alpine',
    'debian',
  ]
  versions = [
    '9.4',
    '9.5',
    '9.6',
    '10',
    '11',
  ]

  versions.each do |version|
    variants.each do |variant|
      if variant == 'debian' then
        fromvariant = ''
        dirvariant = ''
      else
        fromvariant = "-#{variant}"
        dirvariant = "/#{variant}"
      end
      sh "mkdir -p #{version}#{dirvariant}"
      sh "cat Dockerfile-#{variant}.template | sed -e 's/%%SOURCE-TAG%%/#{version}#{fromvariant}/' > #{version}#{dirvariant}/Dockerfile"
      sh "cp -f docker-entrypoint.sh.patch #{version}#{dirvariant}/"
      Dir.chdir("#{version}#{dirvariant}") do
        sh "docker build -t bcit/openshift-postgres:#{version}#{fromvariant} ."
        sh "docker push bcit/openshift-postgres:#{version}#{fromvariant}"
      end
    end
  end
end
