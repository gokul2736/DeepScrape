# DeepScrape-LMS/Portal hacks 


## Disable Copy/Paste function for Portal Discussions
javascript
```
window.addEventListener('paste', (e) => e.stopPropagation(), true);
document.querySelectorAll('*').forEach(el => { el.onpaste = null; });
```

## Disable only Paste Function for Portal Discussions
```
javascript:(function(){window.addEventListener('paste',function(e){e.stopPropagation();},true);document.querySelectorAll('*').forEach(el=>{el.onpaste=null;el.oncopy=null;el.oncut=null;});alert('Paste unblocked!');})();
```

 ## Main Code for Questions Scraping
JavaScript  
```
javascript:(async()=>{const n=document.querySelectorAll('.qnbutton'),t=n.length,b=window.location.href.split('&page=')[0];if(!t)return;console.clear();for(let i=0;i<t;i++){try{const r=await fetch(`${b}&page=${i}`),h=await r.text(),d=new DOMParser().parseFromString(h,'text/html'),q=d.querySelector('.que');if(q){const x=q.querySelector('.qtext').innerText.trim(),s=Array.from(q.querySelectorAll('.flex-fill.ml-1')).map(a=>a.innerText.trim());console.log(`Q${i+1}: ${x}\n${s.map((v,j)=>`  ${String.fromCharCode(97+j)}) ${v}`).join('\n')}\n\n`)}}catch(e){}await new Promise(r=>setTimeout(r,150))}})();
```


## Code for Console (methord 2) api calling 
 ```javascript
(async () => {
    const navButtons = document.querySelectorAll('.qnbutton');
    const totalQuestions = navButtons.length;
    const baseUrl = window.location.href.split('&page=')[0];
    
    console.log(`Starting... Found ${totalQuestions} questions.`);

    let finalOutput = "";

    for (let i = 0; i < totalQuestions; i++) {
        try {
            const response = await fetch(`${baseUrl}&page=${i}`);
            const html = await response.text();
            const doc = new DOMParser().parseFromString(html, 'text/html');
            
            const qContainer = doc.querySelector('.que');
            if (qContainer) {
                const qText = qContainer.querySelector('.qtext').innerText.trim();
                const options = Array.from(qContainer.querySelectorAll('.flex-fill.ml-1'))
                                     .map(opt => opt.innerText.trim());
                
                finalOutput += `Q${i + 1}: ${qText}\n`;
                finalOutput += options.map((o, idx) => `  ${String.fromCharCode(97 + idx)}) ${o}`).join('\n') + "\n\n";
            }
        } catch (e) {
            finalOutput += `Q${i + 1}: Error loading question\n\n`;
        }
        // Minimal delay to keep the connection stable
        await new Promise(r => setTimeout(r, 150));
    }

    // Final print to console
    console.clear();
    console.log(finalOutput);
})();
```
