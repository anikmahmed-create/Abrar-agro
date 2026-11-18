# Abrar-agro<!DOCTYPE html>
<html lang="bn">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Abrar Agro</title>
<style>
body{font-family:Arial,sans-serif;background:#f1f5ef;margin:0;color:#263629}
header{background:#176b38;color:white;padding:22px}
header h1{margin:0}
nav{display:flex;flex-wrap:wrap;background:#fff;padding:8px;gap:6px}
button{padding:12px;border:0;border-radius:8px;background:#176b38;color:white}
main{padding:15px}
.card{background:white;padding:16px;margin:12px 0;border-radius:12px;box-shadow:0 2px 7px #ddd}
input,select{width:100%;padding:12px;margin:7px 0;box-sizing:border-box;border:1px solid #ccc;border-radius:7px}
h2{color:#176b38}
.small{color:#666;font-size:13px}
</style>
</head>
<body>

<header>
<h1>🌱 Abrar Agro</h1>
<p>আমাদের কৃষি, আমাদের ভবিষ্যৎ</p>
</header>

<nav>
<button onclick="show('home')">🏠 হোম</button>
<button onclick="show('plots')">🌱 জমি ও প্লট</button>
<button onclick="show('expenses')">💰 খরচ</button>
<button onclick="show('history')">📚 ইতিহাস</button>
</nav>

<main id="app"></main>

<script>
let plots=JSON.parse(localStorage.getItem('plots')||'[]');
let expenses=JSON.parse(localStorage.getItem('expenses')||'[]');

function save(){
 localStorage.setItem('plots',JSON.stringify(plots));
  localStorage.setItem('expenses',JSON.stringify(expenses));
  }

  function show(page){
   let app=document.getElementById('app');

    if(page==='home'){
     app.innerHTML=`
      <h2>স্বাগতম Abrar Agro</h2>
       <div class="card">🌱 মোট প্লট: ${plots.length}</div>
        <div class="card">💰 মোট খরচ: ৳${expenses.reduce((a,b)=>a+Number(b.amount),0)}</div>
         <div class="card">📚 সংরক্ষিত ফসলের ইতিহাস: ${plots.reduce((a,p)=>a+(p.history||[]).length,0)}</div>
          <p class="small">আপনার খামারের হিসাব এখানে সংরক্ষিত হবে।</p>`;
           }

            if(page==='plots'){
             app.innerHTML=`
              <h2>🌱 নতুন প্লট তৈরি করুন</h2>
               <div class="card">
                <input id="name" placeholder="প্লটের নাম">
                 <input id="area" type="number" placeholder="জমির পরিমাণ (শতাংশ)">
                  <select id="crop">
                   <option>আনারস</option><option>কলা</option>
                    <option>ভুট্টা</option><option>ধান</option>
                     <option>অন্যান্য</option>
                      </select>
                       <input id="date" type="date">
                        <button onclick="addPlot()">প্লট সংরক্ষণ</button>
                         </div>
                          <h2>আমার প্লটসমূহ</h2>
                           ${plots.map((p,i)=>`
                            <div class="card">
                             <h3>${p.name}</h3>
                              জমি: ${p.area} শতাংশ<br>
                               ফসল: ${p.crop}<br>
                                রোপণের তারিখ: ${p.date}<br>
                                 <p>পূর্বের ফসল চক্র: ${(p.history||[]).length}</p>
                                  <button onclick="resetPlot(${i})">নতুন ফসল চক্র শুরু</button>
                                   </div>`).join('')}`;
                                    }

                                     if(page==='expenses'){
                                      app.innerHTML=`
                                       <h2>💰 দৈনিক খরচ</h2>
                                        <div class="card">
                                         <input id="edate" type="date">
                                          <input id="desc" placeholder="খরচের বিবরণ">
                                           <input id="amount" type="number" placeholder="টাকার পরিমাণ">
                                            <button onclick="addExpense()">খরচ সংরক্ষণ</button>
                                             </div>
                                              <h2>খরচের তালিকা</h2>
                                               ${expenses.map(e=>`<div class="card">${e.date} - ${e.desc}<br>৳${e.amount}</div>`).join('')}`;
                                                }

                                                 if(page==='history'){
                                                  app.innerHTML=`
                                                   <h2>📚 প্লটের পুরোনো ইতিহাস</h2>
                                                    ${plots.map(p=>`
                                                     <div class="card">
                                                      <h3>${p.name}</h3>
                                                       ${(p.history||[]).map(h=>`
                                                        <p>${h.crop} | ${h.date}</p>`).join('')}
                                                         </div>`).join('')}`;
                                                          }
                                                          }

                                                          function addPlot(){
                                                           let name=document.getElementById('name').value;
                                                            let area=document.getElementById('area').value;
                                                             let crop=document.getElementById('crop').value;
                                                              let date=document.getElementById('date').value;
                                                               if(!name||!area||!date){alert('সব তথ্য পূরণ করুন');return}
                                                                plots.push({name,area,crop,date,history:[]});
                                                                 save();show('plots');
                                                                 }

                                                                 function resetPlot(i){
                                                                  let p=plots[i];
                                                                   if(!confirm('নতুন ফসল চক্র শুরু করবেন? পুরোনো ইতিহাস থাকবে।'))return;
                                                                    p.history.push({crop:p.crop,date:p.date});
                                                                     let crop=prompt('নতুন ফসলের নাম লিখুন');
                                                                      if(!crop)return;
                                                                       p.crop=crop;
                                                                        p.date=new Date().toISOString().slice(0,10);
                                                                         save();show('plots');
                                                                         }

                                                                         function addExpense(){
                                                                          let date=document.getElementById('edate').value;
                                                                           let desc=document.getElementById('desc').value;
                                                                            let amount=document.getElementById('amount').value;
                                                                             if(!date||!desc||!amount){alert('সব তথ্য পূরণ করুন');return}
                                                                              expenses.push({date,desc,amount});
                                                                                  save();show('expenses');
                                                                               }

                                                                               show('home');
                                                                               </script>
                                                                               </body>
                                                                               </html>