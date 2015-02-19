require "csv"
require 'gems'
require 'uri'

task default: %w[process]

task :process do
  # Clean the posts dir
  # Read the csv
  # Create data for each line
  # Pull repo owner and name for each gem from Rubygems
  # Output the data
  puts "Hello"
  data = CSV.read("report_data.csv")
  limit = 100
  if limit
    data = data.first(limit)
  end
  data.reverse!
  data.each do |row|
    gem = row[0]
    info = Gems.info( gem )
    repo = parse_info( info )
    unless repo.nil?
      write_file( gem, repo)
      puts repo
    end  
  end
end

def write_file( gem, repo)
template = %{---
layout: post
title:  "#{gem}"
repo:   "#{repo}"
date:   2015-02-18 #{Time.new.strftime("%H:%M:%S")}
---}
filename = "_posts/2015-02-18-" + gem + ".markdown"
File.open(filename, "w") do |f|     
f.write(template)   
end
end

def parse_info( info )
  uri = ""
  if info["homepage_uri"] && info["homepage_uri"].include?("github")
      uri = parse_gh_uri( info["homepage_uri"] )
      if uri.end_with?("/")
        uri[uri.size - 1] = ''
      end
      if uri.include?("/")
        return uri
      end
  end
    
  if info["source_code_uri"] && info["source_code_uri"].include?("github")
      uri = parse_gh_uri( info["source_code_uri"] )
      if uri.end_with?("/")
        uri[uri.size - 1] = ''
      end
      if uri.include?("/")
        return uri
      end
  end
  nil
end

def parse_gh_uri( url )
   uri = URI( url )
   path = uri.path
   path[0] = ''
   path
end