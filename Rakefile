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
    sh "mkdir -p #{version}"
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
      sh "cp docker-entrypoint.sh.patch #{version}#{dirvariant}/"
      Dir.chdir("#{version}#{dirvariant}") do
        sh "docker build -t bcit/postgres:#{version}#{fromvariant} ."
        sh "rm -f docker-entrypoint.sh.patch"
      end
    end
  end

  sh "rm -f Dockerfile"
  sh "rm -f docker-entrypoint.sh"
end
