<template>
    <div id="searchframe">

		<div id="imgholder"><img id="theImg" src="../assets/mascot.png"/></div>
		<p class="tag"><b id="front">Git</b><b id="rear">Sniffer</b></p>
			
			<input type="text" name="ghusername" id="ghusername" class="ghusername" placeholder="Github username...">
			
			
			
			
		<button type="button" class="btn btn-info btn-lg" data-toggle="modal" data-target="#myModal" id="ghsubmitbtn"><i class="fas fa-search"></i></button>
		
		<p>&nbsp;</p>
		<div class="darkmode_toggle"><b class="tag">Switch mode :</b> <button id="dark"><i class="fas fa-adjust"></i></button></div>

		
		<!-- Modal -->
		<div class="modal fade" id="myModal" role="dialog">
			<div class="modal-dialog">
			
			<!-- Modal content-->
			<div class="modal-content">
				<div class="modal-body">
				<div id="ghapidata" style="min-height:250px;" class="clearfix"></div>
				</div>
				<div class="modal-footer">
				<button type="button" class="btn btn-default" data-dismiss="modal" id="close_button">Close</button>
				</div>
			</div>
			
			</div>
		</div>
    </div>
</template>

<script src="https://code.jquery.com/jquery-3.5.1.min.js" integrity="sha256-9/aliU8dGd2tb6OSsuzixeV4y/faTqgFtohetphbbj0=" crossorigin="anonymous"></script>
<script>
export default {
  name: 'GitSniffer',
  props: {}
}

	$(function(){
	$('#ghsubmitbtn').on('click', function(e){
		e.preventDefault();
		$('#ghapidata').html('<lottie-player src="https://assets10.lottiefiles.com/packages/lf20_kJNwM4.json"  background="transparent"  speed="1"  style="width: 200px; height: 200px; margin: 50px auto"  loop  autoplay></lottie-player>');
		
		var username = $('#ghusername').val();
		var requri   = 'https://api.github.com/users/'+username;
		var repouri  = 'https://api.github.com/users/'+username+'/repos';
		
		requestJSON(requri, function(json) {
		if(json.message == "Not Found" || username == '') {
			$('#ghapidata').html("<h4>Error 404 : No Users Found</h4>");
			$('#ghapidata').prepend('<lottie-player src="https://assets1.lottiefiles.com/packages/lf20_QIvVpl.json"  background="transparent"  speed="1"  style="width: 200px; height: 200px; margin: 50px auto;"  loop  autoplay></lottie-player>')
		}
		
		else {
			// else we have a user and we display their info
			var fullname   = json.name;
			var username   = json.login;
			var aviurl     = json.avatar_url;
			var profileurl = json.html_url;
			var location   = json.location;
			var followersnum = json.followers;
			var followingnum = json.following;
			var reposnum     = json.public_repos;
			
			if(fullname == undefined) { fullname = username; }
			
			var outhtml = '<div class="ghcontent"><div class="avi"><a href="'+profileurl+'" target="_blank"><img src="'+aviurl+'" width="80" height="80" alt="'+username+'"></a></div>';
			outhtml = outhtml + '<div id="fullname">'+fullname+' &nbsp; <span class="smallname"><a href="'+profileurl+'" target="_blank" id="profileurl"><span class="dot"></span>&nbsp;@'+username+' &nbsp;</a></span></div></div>';
			outhtml = outhtml + '<div id="followdetails"><span class="card"><span class="number">'+followersnum+'</span><span id="text"> <b>follower</b></span></span><span class="card"><span class="number">'+followingnum+'</span><span id="text"> <b>following</b></span></span><span class="card"><span class="number">'+reposnum+'</span><span id="text"> <b>repos</b></p></span></div>';
			
			outhtml = outhtml + '<div class="repolist clearfix">';
			
			var repositories;
			$.getJSON(repouri, function(json){
			repositories = json;   
			outputPageContent();                
			});          
			
			function outputPageContent() {
			if(repositories.length == 0) { outhtml = outhtml + '<p>No repos :(</p></div>'; }
			else {
				outhtml = outhtml + '<p><strong>Repos :</strong></p> <ul>';
				$.each(repositories, function(index) {
				outhtml = outhtml + '<li><a href="'+repositories[index].html_url+'" target="_blank">'+repositories[index].name + '</a></li>';
				});
				outhtml = outhtml + '</ul></div>'; 
			}
			$('#ghapidata').html(outhtml);
			} // end outputPageContent()
		} // end else statement
		}); // end requestJSON Ajax call
	}); // end click event handler
	
	function requestJSON(url, callback) {
		$.ajax({
		url: url,
		complete: function(xhr) {
			callback.call(null, xhr.responseJSON);
		}
		});
	}
	});

	//Dark mode script
	var darkEnabled = false; 
	var place=document.getElementById("ghusername");
	var tag=document.getElementsByClassName("tag");
	$(document).ready(function() {
			$("#dark").on("click", switchDarkMode);
			$("#reset").on("click", reset);
			}
		);
		
		function switchDarkMode(){
			darkEnabled = !darkEnabled;
			if(darkEnabled){
			$("body").addClass("darkmode");
			$(place).addClass("darkmode_lite");
			$(tag).addClass("lighttext");
			} else {
			$("body").removeClass("darkmode");
			$(place).removeClass("darkmode_lite");
			$(tag).removeClass("lighttext");

			}
		}
		
		function reset(){
			$("body").removeClass("darkmode");
			$(place).removeClass("darkmode_lite");
			$(tag).removeClass("lighttext");
		}
</script>

<style>
    #heart-color {
        color: rgb(255, 86, 80);
        margin: 2px;
    }

    #vue-color {
        color: rgb(47, 255, 127);
        margin: 2px;
    }

    #coffee-color {
        color: rgb(224, 176, 71);
    }
</style>