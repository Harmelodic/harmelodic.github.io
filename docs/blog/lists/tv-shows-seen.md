# TV shows seen

In order of most to least favourite:

<table id="content"></table>

<script>
	fetch("https://storage.googleapis.com/harmelodic-web-static-prod/tvShowsSeen.json")
		.then(response => response.json())
		.then(json => {
			document.getElementById("content").innerHTML = json.map((tvShow, index) => `
				<tr>
					<td width="200">
						<img src="https://storage.googleapis.com/harmelodic-web-static-prod/posters/${tvShow.tconst}.jpg"
							 alt="${tvShow.primary_title}"
						/>
					</td>
					<td>
						<p style="font-size:.8rem; line-height:1.6">
							#${index + 1}: <b>${tvShow.primary_title}</b>
							<br/>
							<i>
								<b>Original Release:</b> ${tvShow.start_year}-${tvShow.end_year ? tvShow.end_year : 'Present'}
								<br/>
								<b>Creator(s):</b> ${tvShow.creators.map(creator => '\n\t' + creator).join(',')}
								<br/>
								<b>Genre(s):</b> ${tvShow.genres.split(',').map(genre => ' ' + genre).join(',')}
							</i>
						</p>
					</td>
				</tr>
			`).join("");
		})
</script>
