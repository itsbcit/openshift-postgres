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

  variants = [
    'alpine',
  ]
  versions = [
    '9.4',
    '9.5',
    '9.6',
    '10',
    '11',
  ]
  patchversion = {
    '9.4' => '9.4.21',
    '9.5' => '9.5.16',
    '9.6' => '9.6.12',
    '10'  => '10.7',
    '11'  => '11.2',
  }

  versions.each do |version|
    variants.each do |variant|
      if variant == 'alpine' then
        fromvariant = ''
        dirvariant = ''
      else
        fromvariant = "-#{variant}"
        dirvariant = "/#{variant}"
      end
      sh "mkdir -p #{version}#{dirvariant}"
      sh "cp -f docker-entrypoint.sh.patch #{version}#{dirvariant}/"
      Dir.chdir("#{version}#{dirvariant}") do
        sh "docker build -t bcit/openshift-postgres:#{version}#{fromvariant} ."
        sh "docker tag bcit/openshift-postgres:#{version}#{fromvariant} bcit/openshift-postgres:#{patchversion[version]}#{fromvariant}"
        sh "docker push bcit/openshift-postgres:#{version}#{fromvariant}"
        sh "docker push bcit/openshift-postgres:#{patchversion[version]}#{fromvariant}"
    render_template("Dockerfile.erb", "#{version}/Dockerfile", binding)
      end
    end
  end
end
