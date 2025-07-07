# Films seen

In order of most to least favourite:

<table id="content"></table>

<script>
	fetch("https://storage.googleapis.com/harmelodic-web-static-prod/filmsSeen.json")
		.then(response => response.json())
		.then(json => {
			document.getElementById("content").innerHTML = json.map((film, index) => `
				<tr>
					<td width="200">
						<img src="https://storage.googleapis.com/harmelodic-web-static-prod/posters/${film.tconst}.jpg"
							 alt="${film.primary_title}"
						/>
					</td>
					<td>
						<p style="font-size:.8rem; line-height:1.6">
							#${index + 1}: <b>${film.primary_title}</b>
							<br/>
							<i>
								<b>Year:</b> ${film.start_year}<br/>
								<b>Director(s):</b> ${film.directors.join(", ")}<br/>
								<b>Writer(s):</b> ${film.writers.map(writer => '\n\t' + writer).join(',')}<br/>
								<b>Genre(s):</b> ${film.genres.split(',').map(genre => ' ' + genre).join(',')}<br/>
								<b>Runtime:</b> ${film.runtime_minutes} minutes
							</i>
						</p>
					</td>
				</tr>
			`).join("");
		})
</script>
