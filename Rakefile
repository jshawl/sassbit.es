task :default do
  puts 'here'
  require 'httparty'
  require 'ox'
  class Fetch
    include HTTParty
    format :xml
  end

  res = Fetch.get('http://gdata.youtube.com/feeds/api/users/sassbites/uploads')
  res['feed']['entry'].each do |p|\
    @title = p['title']['__content__'].split(' - ')[1].downcase.gsub(/[^A-Za-z]/,'-')
    @url = p['link'][0]['href']
    #p @url, @title
    @date = p['published'].split('T')[0]
    @filename = @date + '-' + @title + '.md'
    p @filename
  end
end