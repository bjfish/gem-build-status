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
  limit = 5000
  overrides = { 
     "thor" => {  :repo => "erikhuda/thor"  } # links in rubygems redirected  
  }
  if limit
    data = data.first(limit)
  end
  data.reverse!
  data.each do |row|
    gem = row[0]
    info = Gems.info( gem )
    repo = parse_info( info )
    gemurl = info["homepage_uri"] || info["source_code_uri"] || ""
    geminfo = { :repo => repo, :gem => gem, :gemurl => gemurl }
    if overrides.include?( gem )
       geminfo.merge!( overrides[gem] )
    end   
    unless repo.nil?
      write_file( geminfo)
      puts geminfo[:repo]
    end  
  end
end

def write_file( geminfo )
gem = geminfo[:gem]
repo = geminfo[:repo]
gemurl = geminfo[:gemurl]
template = %{---
layout: post
title:  "#{gem}"
repo:   "#{repo}"
date:   2015-02-18 #{Time.new.strftime("%H:%M:%S")}
gemurl: #{gemurl}
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
      if uri.include?("/") && uri.count("/") == 1
        return uri
      end
  end
    
  if info["source_code_uri"] && info["source_code_uri"].include?("github")
      uri = parse_gh_uri( info["source_code_uri"] )
      if uri.end_with?("/")
        uri[uri.size - 1] = ''
      end
      if uri.include?("/") && uri.count("/") == 1
        return uri
      end
  end
  nil
end

def parse_gh_uri( url )
   path = ""
   begin
     uri = URI( url )
     path = uri.path
     path[0] = ''
   rescue
     path = ""
   end
   path
end