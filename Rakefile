require 'csv'
require 'dotenv'
require 'selenium-webdriver'

Dotenv.load

task default: %w[all]

USERNAME = ENV.fetch('USERNAME', '')
PASSWORD = ENV.fetch('PASSWORD', '')

# see https://github.com/SeleniumHQ/selenium/wiki/Ruby-Bindings
prefs = {
  download: {
    prompt_for_download: false,
    default_directory: "#{Dir.pwd}"
  }
}

@home = 'https://lists.clir.org/cgi-bin/wa?HOME'
@login = 'https://lists.clir.org/cgi-bin/wa?LOGON'
@subscriber_report = 'https://lists.clir.org/cgi-bin/wa?REPORT=DLF-ANNOUNCE&z=2&9=A&9=B&9=I&9=N&X=P73C2C0B6936154BF97'
@browser = Selenium::WebDriver.for(:chrome, prefs: prefs)

task :all => [:clean, :login, :download_report] do

  # puts @browser.page_source

end

task :clean do
  `rm wa-report*`
  # FileUtils.rm_f('wa-report*')
end

task :download_report do
  @browser.get(@subscriber_report)

  select_boxes = @browser.find_elements(tag_name: 'select')
  select_boxes[1].find_elements(tag_name: 'option').each do |element|
    if(element.text == 'CSV Format (All)')
      element.click
      break
    end
  end

  buttons = @browser.find_elements(xpath: "//input[@type='submit'and @value='Submit']")
  puts buttons
  buttons.last.click
end

task :login do
  @browser.get(@login)
  user = @browser.find_element(name: 'Y')
  user.send_keys(USERNAME)
  password = @browser.find_element(name: 'p')
  password.send_keys(PASSWORD)
  @browser.find_element(name: 'e').click
end
