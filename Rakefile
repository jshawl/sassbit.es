task :default do
  require 'httparty'
  require 'pry'
  class Fetch
    include HTTParty
    format :xml
    def self.fetch url, ress = []
      res = self.get url
      ress << res
      begin
	next_url = ''
	res['feed']['link'].each do |l|
	  if l['rel'] == 'next' 
	    next_url = l['href'] if l['href'] != ''
	  end
	end
	if next_url
	  puts "fetching: " + next_url
	  self.fetch next_url, ress
	end
      rescue
      end
      return ress
    end
  end


  res = Fetch.fetch('http://gdata.youtube.com/feeds/api/users/sassbites/uploads')
  p res.length
  res.each do |r|
    r['feed']['entry'].each do |p|\
      @title = p['title']['__content__']
      begin
	@formatted_title = @title.split(' - ')[1].downcase.gsub(/[^A-Za-z]/,'-')
      rescue
	@formatted_title = @title.downcase.gsub(/[^A-Za-z]/,'-')
      end
      @url = p['link'][0]['href']
      @date = p['published'].split('T')[0]
      @filename = '_posts/' +  @date + '-' + @formatted_title + '.md'
      @embed_url = p['id'].split('/')[-1]
      p @title
      unless File.exists? @filename
	f = File.new( @filename, "w+")
	f.write("---\n")
	f.write("layout: post\n")
	f.write("title: \"#{@title}\"\n")
	f.write("---\n\n")
	f.write("<iframe width='560' height='315' src='//www.youtube.com/embed/#{@embed_url}' frameborder='0' allowfullscreen></iframe>")
	f.close
      end
    end
  end
end