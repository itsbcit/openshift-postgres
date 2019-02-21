desc "Build all images"
task :default do
  sh "curl -O https://raw.githubusercontent.com/docker-library/postgres/master/docker-entrypoint.sh"
  sh "patch -p0 < docker-entrypoint.sh.patch"

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
      else
        fromvariant = "-#{variant}"
      end
      sh "cat Dockerfile-#{variant}.template | sed -e 's/%%SOURCE-TAG%%/#{version}#{fromvariant}/' > Dockerfile"
      sh "docker build -t bcit/postgres:#{version}#{fromvariant} ."
    end
  end
  
  sh "rm -f Dockerfile"
  sh "rm -f docker-entrypoint.sh"
end
