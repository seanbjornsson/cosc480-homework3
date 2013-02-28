# Add a declarative step here for populating the DB with movies.

Given /the following movies exist/ do |movies_table|
  movies_table.hashes.each do |movie|
   Movie.create(movie)
  end
end

# Make sure that one string (regexp) occurs before or after another one
#   on the same page

Then /I should see "(.*)" before "(.*)"/ do |e1, e2|
	page.body =~ /#{e1}.*#{e2}/m
end

# Make it easier to express checking or unchecking several boxes at once
#  "When I uncheck the following ratings: PG, G, R"
#  "When I check the following ratings: G"

When /I (un)?check the following ratings: (.*)/ do |uncheck, rating_list|
	rating_list.gsub(/,/, ' ').split(' ').each do |rating|
		if uncheck == 'un' 
			step %{I uncheck "ratings_#{rating}"}
		else
			step %{I check "ratings_#{rating}"}
		end
	end
end

When /I check all ratings/ do
	Movie.all_ratings.each do |rating|
		step %{I check "ratings_#{rating}"}
	end
end

Then /I should see all of the movies/ do
	rows = (page.all('table#movies tr').count-1)
 print "number of rows in the table = #{rows}\n"
	print "number of movies in db = #{Movie.count}"
	(page.all('table#movies tr').count-1).should == Movie.count
end

Then /I should (not )?see movies with the following ratings: (.*)/ do |show, rating_list|
	Movie.find_all_by_rating(rating_list.gsub(/,/, ' ').split(' ')).each do |movie|
		if show == 'not '	
			step %{I should not see "#{movie[:title]}"}
		else
			step %{I should see "#{movie[:title]}"}
		end
	end
end

Then /the movies should be sorted (alphabetically|by release date)/ do |order|
	if order == 'alphabetically'
		sort = 'title'
	else
		sort = 'release_date'
	end
	movies = Movie.order(sort).select("title")
	0.upto(movies.length-2) do |i|
		print "movies being compared: #{movies[i]["title"]} and #{movies[i+1]["title"]}\n"
		step %{I should see "#{movies[i]["title"]}" before "#{movies[i+1]["title"]}"}
	end
end








