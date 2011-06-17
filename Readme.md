# 960 Grid in Stylus

A port of the static and fluid 960 grid systems to the stylus language.
This project is based on Nathan Smith's work at [960.gs][1], Chris 
Eppstein's work porting the 960 grid system to the [Compass][2] framework 
and the [fluid 960 grid system][4] project by Stephen Bau.

This port was done with the primary goal of providing a quick way to 
map semantic markup into a grid by using an intermediary stylesheet that
overrides variable defaults where appropriate and leverages the 960 grid
mixins to apply grid css.

Also included are general intermediaries, files which you can compile from
the command line to generate static grid stylesheets.

stylus_grid also includes a basic css reset which can be used to normalize
platform inconsistencies, allowing you to build up your stylesheets from
a consistent baseline.


### Project Structure

	+ grid_stylus
		- reset.styl
		+ 960
			- grid.styl			// general intermediary
			- _grid.styl		// 960 grid mixins
		+ fluid
			- grid.styl			// general intermediary
			- _grid.styl		// fluid 960 mixins

Within the grid_stylus project folder you'll find a stylus implementation
of a baseline css reset along with directories for both the static (fixed)
and fluid 960 grids.

Within each of the grid directories there is a _grid.styl file containing 
the mixins, variables and base styles for the 960 grid as well as a 
grid.styl file which compiles into a full static 960 grid stylesheet
that can be included in your sites.


### Quick Start

You can get started quickly by cloning the project and compiling either
the fixed or fluid grids to wherever you want them:

	#> git clone git://github.com/patrickdanger/grid_stylus.git
	#> cd grid_stylus
	#> stylus 960/grid.styl -o path/to/your/stylesheets
	
I recommend compressing the output when compiling the complete grid
stylesheets as they can be pretty big (compression buys you 4k):

	#> stylus fluid/grid.styl -c -o path/to/your/stylesheets


### Not-so-quick Start with an Intermediary

Creating a semantic intermediary decouples the markup in your pages 
from the styles in the grid stylesheets.  Essentially it frees you up
from having to use the grid classes directly in your markup which 
allows you to either use existing classes and attributes or to eschew
all that nonsense and just map your semantic structure into the grid.

Consider the following standard (:P) page setup.  Notice the elements
classed as "clear" below, these are a vestige of platform normalization
in the original 960.gs codebase and I'm sure anyone who has used the
960 grid before will recognize them.

	<!DOCTYPE html>
	<html>
		<head><title>Example!</title></head>
		<body>
			<div id="primarylayout">
				<h1 id="pageheader">Example!</h1>
				<span class="clear"></span>
				
				<div id="content">Some Content ...</div>
				<div id="sidebar">Sidebar info ...</div>
				<span class="clear"></span>
			</div>
		</body>
	</html>

Below is an example of an intermediate stylesheet that maps these common
elements into a 16 column grid layout with 20 pixel gutters and a width
of 960 pixels (the basic 960.gs setup):

	@import "grid_stylus/_reset.styl"
	@import "grid_stylus/960/_grid.styl"

	var_grid_columns 16
	var_grid_gutter 20px
	var_grid_width 960px

	div#primarylayout
		container()
		#pageheader
			grid_(16)
		
	div#content
		grid_(11)
	
	div#sidebar
		grid_(5)


If you changed your mind and decided that a 12 column layout was more to 
your liking:

	@import "grid_stylus/_reset.styl"
	@import "grid_stylus/960/_grid.styl"

	var_grid_columns 12
	var_grid_gutter 20px
	var_grid_width 960px

	div#primarylayout
		container()
		#pageheader
			grid_(12)
	
	div#content
		grid_(8)

	div#sidebar
		grid_(4)



[1]: http://960.gs
[2]: http://compass-style.org/
[3]: http://www.designinfluences.com/
[4]: https://github.com/bauhouse/fluid960gs