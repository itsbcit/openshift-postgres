require "erb"

# @ripienaar https://www.devco.net/archives/2010/11/18/a_few_rake_tips.php
# Brilliant.
def render_template(template, output, scope)
    tmpl = File.read(template)
    erb = ERB.new(tmpl, 0, "<>")
    File.open(output, "w") do |f|
        f.puts erb.result(scope)
    end
end

desc "Update Dockerfile templates"
task :default do

  versions = [
    '11',
    '10',
    '9.6',
    '9.5',
    '9.4',
  ]
  extratags = {
    '11'  => ['latest', 'alpine', '11.2',    '11-alpine', '11.2-alpine'],
    '10'  => [                    '10.7',    '10-alpine', '10.7-alpine'],
    '9.6' => [               '9',  '9.6.12',  '9-alpine',  '9.6-alpine', '9.6.12-alpine'],
    '9.5' => [                     '9.5.16',               '9.5-alpine', '9.5.16-alpine'],
    '9.4' => [                     '9.4.21',               '9.4-alpine', '9.4.21-alpine'],
  }
  extratags.default = []

  versions.each do |version|
    sh "mkdir -p #{version}"
    render_template("Dockerfile.erb", "#{version}/Dockerfile", binding)
    sh "cp -f docker-entrypoint.sh.patch #{version}/"
    Dir.chdir("#{version}") do
      sh "docker build -t bcit/openshift-postgres:#{version} ."
      sh "docker push bcit/openshift-postgres:#{version}"

      extratags[version].each do |extratag|
        sh "docker tag bcit/openshift-postgres:#{version} bcit/openshift-postgres:#{extratag}"
        sh "docker push bcit/openshift-postgres:#{extratag}"
      end
    end
  end
end
