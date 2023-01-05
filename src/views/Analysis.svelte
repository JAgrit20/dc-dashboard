<div style="
display: flex;
justify-content: space-around;
">

  <div style="
  height: 500px;
  width: 400px;
  padding-left: 20px;
  ">
    <canvas id="layanan" width="500px" height="400px"></canvas>
  </div>
  
  <div style="
  height: 500px;
  width: 400px;
  padding-left: 20px;
  ">
    <canvas id="layanan_subbagian" width="500px" height="400px"></canvas>
  </div>
</div>
<div style="
display: flex;
justify-content: space-around;
">
<div style="
height: 500px;
width: 400px;
padding-left: 20px;
">
	<canvas id="layanan_subbagian2" width="500px" height="400px"></canvas>
</div>
<div style="
height: 500px;
width: 400px;
padding-left: 20px;
">
	<canvas id="layanan_subbagian3" width="500px" height="400px"></canvas>
</div>
</div>
<div style="
display: flex;
justify-content: space-around;
">
<div style="
height: 500px;
width: 400px;
padding-left: 20px;
">
	<canvas id="layanan_subbagian4" width="500px" height="400px"></canvas>
</div>
<div style="
height: 500px;
width: 400px;
padding-left: 20px;
">
	<canvas id="layanan_subbagian5" width="500px" height="400px"></canvas>
</div>
</div>
<div style="
display: flex;
justify-content: space-around;
">
<div style="
height: 500px;
width: 400px;
padding-left: 20px;
">
	<canvas id="layanan_subbagian6" width="500px" height="400px"></canvas>
</div>
<div style="
height: 500px;
width: 400px;
padding-left: 20px;
">
	<canvas id="layanan_subbagian7" width="500px" height="400px" style="display: block;box-sizing: border-box;height: 400px !important;width: 550px !important;"></canvas>
</div>
</div>
<div style="
display: flex;
justify-content: space-around;
">
<div style="
height: 500px;
width: 400px;
padding-left: 20px;
">
	<canvas id="layanan_subbagian8" width="600px" height="500px" style="display: block;box-sizing: border-box;height: 400px !important;width: 550px !important;"></canvas>
</div>
<div style="
height: 600px;
width: 500px;
padding-left: 20px;
">
	<canvas id="layanan_subbagian9" width="600px" height="500px"></canvas>
</div>
</div>
<script>
	import Accordion from '../components/Accordion.svelte'
	import AccordionHeader from '../components/AccordionHeader.svelte'
	import AccordionButton from '../components/AccordionButton.svelte'
	import AccordionBody from '../components/AccordionBody.svelte'
  import {
    MapChart,
    Geographies,
    Geography,
  } from "../../node_modules/svelte-mapchart/src/components/components.module";
 
    let geoUrl = "https://cdn.jsdelivr.net/npm/world-atlas@2/countries-110m.json";


	let teachers = []
	let teachersphone = []
	let promise = getTeachers()

	import {onMount} from 'svelte' 
 import Chart from 'chart.js/auto'
  let ctx;
  let ctx2;


	async function getTeachers() {
		const res = await fetch('/api/phase2')
		const data = await res.json()

		if (!res.ok)
			throw new Error('Request to server was unsuccessful.')

		if (data == null)
			throw new Error('No data received from server.')

		teachers = []
		teachersphone = []

		Object.keys(data).forEach(uid => {
		
			teachers = [
				...teachers,
				{
					...data[uid],
					
					hKey: `h-p2--${uid}`,
					cKey: `c-p2--${uid}`,
					confirmChange: false
				}
			]
			
		})
		console.log(teachers)
		return true
	}

	onMount(
	async () =>{
		var ctx = document.getElementById("layanan").getContext('2d');
            var data = {
                datasets: [{
                    data: [10, 20, 30, 40, 25, 32, 45],
                    backgroundColor: [
                        '#3c8dbc',
                        '#f56954',
                        '#f39c12',
						'#C9ED13',
						'#139EED',
						'#A274EA',
						'#EA749F'
                    ],
                }],
                labels: [
                    'Monday',
                    'Tuesday',
                    'Wednesday',
					'Thursday',
					'Friday',
					'Saturday',
					'Sunday'				
                ]
            };
            var myDoughnutChart = new Chart(ctx, {
                type: 'bar',
                data: data,
                options: {
                    title: {
        display: true,
        text: 'Students signed up',
      },
                    responsive: false,
                    maintainAspectRatio: false,
                    legend: {
                        position: 'bottom',
                        labels: {
                            boxWidth: 12
                        },
                    },
                },
            });

            var ctx_2 = document.getElementById("layanan_subbagian").getContext('2d');
            var data_2 = {
                datasets: [{
                    data: [10, 20, 30],
                    backgroundColor: [
                        '#3c8dbc',
                        '#f56954',
                        '#f39c12',
                    ],
                }],
                labels: [
                    'Not able to understand',
                    'Video breaking',
                    'Voice not clear'
                ]
            };
            var myDoughnutChart_2 = new Chart(ctx_2, {
                type: 'pie',
                data: data_2,
                options: {
                    responsive: false,
                    maintainAspectRatio: false,
                    title: {
        display: true,
        text: 'Problems faced',
      },
                    legend: {
                        position: 'bottom',
                        labels: {
                            boxWidth: 12
                        },
                    },
                },
            });
            var ctx_3 = document.getElementById("layanan_subbagian2").getContext('2d');
            var data_3 = {
                datasets: [{
                    data: [10, 20, ],
                    backgroundColor: [
                        '#3c8dbc',
                        '#f56954'
                    ],
                }],
                labels: [
                    'More than 3 Stars',
                    'Less than 3 stars',
                ]
            };
            var myDoughnutChart_3 = new Chart(ctx_3, {
                type: 'doughnut',
                data: data_3,
                options: {
                    responsive: false,
                    maintainAspectRatio: false,
                    title: {
        display: true,
        text: 'Rating System'
      },
                    legend: {
                        position: 'bottom',
                        labels: {
                            boxWidth: 12
                        },
                    },
                },
            });
			var ctx_4 = document.getElementById("layanan_subbagian3").getContext('2d');
            var data_4 = {
                datasets: [{
                    data: [10, 30, 40, 45],
                    backgroundColor: [
                        '#3c8dbc',
                        '#f56954',
                        '#f39c12',
						'#C9ED13',
                    ],
                }],
                labels: [
                    'Bilology',
                    'Physics',
                    'Chemistry',
					'Mathematics',			
                ]
            };
            var myDoughnutChart_4 = new Chart(ctx_4, {
                type: 'bar',
                data: data_4,
                options: {
                    responsive: false,
                    maintainAspectRatio: false,
                    title: {
        display: true,
        text: 'Most preferred Subject'
      },
                    legend: {
                        position: 'bottom',
                        labels: {
                            boxWidth: 12
                        },
                    },
                },
            });
			var ctx_5 = document.getElementById("layanan_subbagian4").getContext('2d');
			var data_5 = {
                datasets: [{
                    data: [10, 30, 40, 45, 60, 90, 10, 65, 40, 36, 87, 43],
                    borderColor: [
                        '#3c8dbc'
                    ],
					tension: '0.1'
                }],
                labels: [
                    'January',
                    'February',
                    'March',
					'April',
					'May',
					'June',
					'July',
					'August',
					'September',
					'October',
					'November',
					'December'			
                ]
            };
            var myDoughnutChart_5 = new Chart(ctx_5, {
                type: 'line',
                data: data_5,
                options: {
                    responsive: false,
                    maintainAspectRatio: false,
                    title: {
        display: true,
        text: 'Average session duration'
      },
                    legend: {
                        position: 'bottom',
                        labels: {
                            boxWidth: 12
                        },
                    },
                },
            });
			var ctx_6 = document.getElementById("layanan_subbagian5").getContext('2d');
			var data_6 = {
                datasets: [{
                    data: [10, 15, 30, 25, 54, 67, 43, 32, 76, 89, 62, 56],
                    borderColor: [
                        '#f56954'
                    ],
					tension: '0.1'
                }],
                labels: [
                    'January',
                    'February',
                    'March',
					'April',
					'May',
					'June',
					'July',
					'August',
					'September',
					'October',
					'November',
					'December'			
                ]
            };
            var myDoughnutChart_6 = new Chart(ctx_6, {
                type: 'line',
                data: data_6,
                options: {
                    responsive: false,
                    maintainAspectRatio: false,
                    title: {
        display: true,
        text: 'Monthly User Retention'
      },
                    legend: {
                        position: 'bottom',
                        labels: {
                            boxWidth: 12
                        },
                    },
                },
            });
			var ctx_7 = document.getElementById("layanan_subbagian6").getContext('2d');
			var data_7 = {
                datasets: [{
                    data: [10, 30, 54, 43, 76, 62, 56],
                    borderColor: [
                        '#A274EA'
                    ],
					tension: '0.1'
                }],
                labels: [
                    'Monday',
                    'Tuesday',
                    'Wednesday',
					'Thursday',
					'Friday',
					'Saturday',
					'Sunday'	
                ]
            };
            var myDoughnutChart_7 = new Chart(ctx_7, {
                type: 'line',
                data: data_7,
                options: {
                    responsive: false,
                    maintainAspectRatio: false,
                    title: {
                      display: true,
                    text: 'Returning Users'
                    },
                    legend: {
                        position: 'bottom',
                        labels: {
                            boxWidth: 12
                        },
                    },
                },
            });
			var ctx_8 = document.getElementById("layanan_subbagian7").getContext('2d');
			var labels = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December' ];
            var data_8 = {
  labels: labels,
  datasets: [
    {
      label: 'Signups',
      data: [65, 78, 63, 89, 56, 21, 54, 38, 97, 65, 45, 32],
      borderColor: '#3c8dbc',
      backgroundColor: '#3c8dbc',
    },
    {
      label: 'Doubts',
      data: [32, 24, 5, 45, 32, 10, 21, 22, 67, 43, 31, 9],
      borderColor: '#f56954',
      backgroundColor: '#f56954',
    }
  ]
};
            var myDoughnutChart_8 = new Chart(ctx_8, {
                type: 'bar',
                data: data_8,
  options: {
    responsive: false,
    maintainAspectRatio: false,
    indexAxis: 'y',
    elements: {
      bar: {
        borderWidth: 2,
      }
    },
    plugins: {
      legend: {
        position: 'bottom',
      },
      title: {
        display: true,
        text: 'Sign up to doubt rate'
      },
    },
},       
            });
            var ctx_9 = document.getElementById("layanan_subbagian8").getContext('2d');
			var labels = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December' ];
            var data_9 = {
  labels: labels,
  datasets: [
    {
      label: 'Total IDs',
      data: [65, 78, 63, 89, 56, 21, 54, 38, 97, 65, 45, 32],
      borderColor: '#C9ED13',
      backgroundColor: '#C9ED13',
    },
    {
      label: 'Unique IDs',
      data: [32, 24, 5, 45, 32, 10, 21, 22, 67, 43, 31, 9],
      borderColor: '#EA749F',
      backgroundColor: '#EA749F',
    }
  ]
};
            var myDoughnutChart_9 = new Chart(ctx_9, {
                type: 'bar',
                data: data_9,
  options: {
    indexAxis: 'y',
    elements: {
      bar: {
        borderWidth: 2,
      }
    },
    responsive: false,
    maintainAspectRatio: false,
    plugins: {
      legend: {
        position: 'bottom',
      },
      title: {
        display: true,
        text: 'Doubts asked per month'
      },
    },
},       
            });
            var ctx_10 = document.getElementById("layanan_subbagian9").getContext('2d');
			var labels = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December' ];
            var data_10 = {
  labels: labels,
  datasets: [
    {
      label: 'Returning Ids',
      data: [65, 78, 63, 89, 56, 21, 54, 38, 97, 65, 45, 32],
      borderColor: '#C9ED13',
      backgroundColor: '#C9ED13',
    },
    {
      label: 'Unique IDs',
      data: [32, 24, 5, 45, 32, 10, 21, 22, 67, 43, 31, 9],
      borderColor: '#EA749F',
      backgroundColor: '#EA749F',
    }
  ]
};
var myDoughnutChart_10 = new Chart(ctx_10, {
                type: 'line',
                data: data_10,
  options: {
    responsive: true,
    plugins: {
      legend: {
        position: 'bottom',
      },
      title: {
        display: true,
        text: 'Unique vs Returning users each month'
      },
    },
},       
            });
	}
);
	const putPhase2 = (uid) => {
		fetch(`/api/phase2?uid=${uid}`, {
			method: 'PUT'
		})
		.then(res => {
			if (res.status === 200) {
				alert('Success, Refresh the page for double confirmation.')
			} else alert('Failure')
		})
		.catch(err => alert(`Failure! Message: ${err.message}`))
	}

	const refresh = () => { promise = getTeachers() }
</script>
<main>
  <MapChart projection="geoEqualEarth">
        <Geographies geography={geoUrl} let:features={features}>
            {#each features as feature}
                <Geography feature={feature} />
            {/each}
        </Geographies>
    </MapChart>
</main>

<button class="btn btn-secondary" on:click={refresh}>Refresh</button>
{#await promise}
	<p>Requesting . . . </p>
{:then success}
<Accordion id="phase2-accordion">
	{#each teachers as teacher (teacher.uid)}
		<div class="card">
			<AccordionHeader id={teacher.hKey} target={teacher.cKey}>
				{teacher.firstname} {teacher.lastname} - Phase {teacher.phase}
			</AccordionHeader>
			<AccordionBody id={teacher.hKey} target={teacher.cKey} parent="#phase2-accordion">
				<strong>UID: </strong>{teacher.uid}<br>
				<strong>Name: </strong>{teacher.firstname} {teacher.lastname}<br>
				<strong>Email: </strong>{teacher.Email}<br>
				<strong>Phone Number: </strong>{teacher.phone}<br>
				<strong>D.O.B: </strong>{teacher.dob}<br>
				<strong>Rate per doubt: </strong>{teacher.rate_per_doubt}<br>
				<strong>Primary Language: </strong>{teacher.lang}<br>
				<strong>Tags: </strong><br>
				<ul>
					{#each teacher.tags.split(", ") as tag (tag)}
						<li>{tag}</li>
					{/each}				
				</ul>

				<button 
				  class="btn btn-primary m-3 px-1 py-2" 
				  disabled={teacher.confirmChange} 
				  on:click={() => teacher.confirmChange = true}
				>Shift teacher to phase 3</button>
				{#if teacher.confirmChange}
					<button 
					  class="btn btn-success m-3 px-1 py-2"
					  on:click={() => {
					  	teacher.confirmChange = false
						putPhase2(teacher.uid)
					  }}
					>Confirm</button>
					<button
					  class="btn btn-danger m-3 px-1 py-2"
					  on:click={() => teacher.confirmChange = false}
					>Cancel</button>
				{/if}
		
		
			</AccordionBody>
		
			
		</div>
	{:else}
		<strong class="text-center">No Teacher is in phase 2 currently</strong>
	{/each}
</Accordion>
{:catch err}
	<p style="color: red;">{err.message}</p>
{/await}
