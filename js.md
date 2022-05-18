> > 声明：本页面仅供JS脚本学习、交流和讨论，请勿使用它们进行非法勾当。
> 
> ## 腾讯视频
> 
> ```js
> var a=prompt(PLAYER._DownloadMonitor.context.dataset.title,PLAYER._DownloadMonitor.context.dataset.ckc?PLAYER._DownloadMonitor.context.dataset.currentVideoUrl:PLAYER._DownloadMonitor.context.dataset.currentVideoUrl.replace(/:.*qq.com/g,"://defaultts.tc.qq.com/defaultts.tc.qq.com"));
> ```
> 
> ## 腾讯视频(DRM内容)
> 
> ```js
> var download = function (text, filename) {
>     let b = new Blob([text], {
>       type: 'text/plain'
>     });
>     let a = document.createElement("a");
>     a.href = URL.createObjectURL(b);
>     a.setAttribute("download", filename);
>     a.click();
> };
> var m3u8Content=PLAYER._DownloadMonitor.context.dataset.playList[0].m3u8;
> var title=PLAYER._DownloadMonitor.context.dataset.title+"[v+a].m3u8";
> download(m3u8Content, title);
> ```
> 
> ## 爱奇艺(iqiyi.com)视频
> 
> ```js
> var download = function (text, filename) {
>     let b = new Blob([text], {
>       type: 'text/plain'
>     });
>     let a = document.createElement("a");
>     a.href = URL.createObjectURL(b);
>     a.setAttribute("download", filename);
>     a.click();
> };
> var importCmd5xAsync = async function() {
>     let resp = await fetch("https://static.iqiyi.com/js/common/f6a3054843de4645b34d205a9f377d25.js");
>     let jsCode = await resp.text();
>     let script = document.createElement("script");
>     script.text = jsCode;
>     document.getElementsByTagName("head")[0].appendChild(script);
> };
> var fetchFlvAsync = async function(info) {
>     var fs = info.fs;
>     var content = "#EXTM3U\n";
>     var prefix = "https://data.video.iqiyi.com/videos";
>     let results = fs.map(async fs_i => {
>         let url = fs_i.l;
>         let  api = prefix + url;
>         let t = "";
>         if(playerObject._player.package.engine.adproxy.engine.movieinfo.current.boss){
>             t = playerObject._player.package.engine.adproxy.engine.movieinfo.current.boss.data.t;
>         }
>         api = prefix + url + "%E2%9C%97domain=1&t=" + t + "&QY00001=" + /qd_uid=(\d+)/g.exec(url)[1] + "&ib=4&ptime=0&ibt=" + cmd5x(t + /\/(\w{10,})/g.exec(url)[1]);
>         let resp = await fetch(api,{credentials: 'same-origin'});
>         let json = await resp.json();
>         return "#EXTINF:"+(fs_i.d/1000).toFixed(2)+"\n" + json["l"];
>     });
>     let urls = await Promise.all(results);
>     content += urls.join('\n');
>     content += "\n#EXT-X-ENDLIST";
>     return content;
> };
> var ndIqyAsync = async function() {
>     var vTracks = playerObject._player.package.engine.adproxy.engine.movieinfo.current.vidl.filter(a=>a.isUsable).sort((a,b)=>b.vsize-a.vsize);
>     let p = "";
>     vTracks.forEach(function (item, index) {
>         p += `\r\n[${index}]: [${item.realArea.width}x${item.realArea.height}]_${(item.vsize / 1024 / 1024).toFixed(2)}MB`;
>     });
>     let _input = prompt("请选择视频" + p);
>     if (!_input) return;
>     let info = vTracks[Number(_input)];
>     var m3u8Content = "";
>     if(typeof info.playlist == "string" && info.playlist.length>0){
>         m3u8Content = info.playlist;
>     }else if(typeof info.playlist == "object"){
>         m3u8Content = "#EXTM3U\n" + info.playlist.urls.map((u,i)=>"#EXTINF:"+info.playlist.durations[i]+",\nhttps:"+u+info.playlist.qdp[/\/(\w{10,})/g.exec(u)[1]]).join('\n') + "\n#EXT-X-ENDLIST";
>     }else{
>         await importCmd5xAsync();
>         m3u8Content = await fetchFlvAsync(info);
>     }
>     var title = "";
>     try{
>         title = document.querySelector('h1.player-title a.title-link[title]')['title'] + "_" + document.querySelector('h1.player-title em').innerText + "_" + info.realArea.width + "_" + info.realArea.height + "_" + (info.code == 2 ? "H264": "H265") + "_" + document.getElementsByClassName("iqp-time-dur")[0].innerText.replace(/:/, ".") + "_" + (info.vsize / 1024 / 1024).toFixed(2) + "MB.m3u8";
>     } catch(err){
>         title = document.title + "_" + info.realArea.width + "_" + info.realArea.height + "_" + (info.code == 2 ? "H264": "H265") + "_" + document.getElementsByClassName("iqp-time-dur")[0].innerText.replace(/:/, ".") + "_" + (info.vsize / 1024 / 1024).toFixed(2) + "MB.m3u8";
>     } 
>     if(m3u8Content!="") download(m3u8Content, title);
> };
> ndIqyAsync();
> ```
> 
> ## 爱奇艺(iq.com)视频
> 
> ```js
> var download = function (text, filename) {
>     let b = new Blob([text], {
>       type: 'text/plain'
>     });
>     let a = document.createElement("a");
>     a.href = URL.createObjectURL(b);
>     a.setAttribute("download", filename);
>     a.click();
> };
> var ndIq = function() {
>     var info = playerObject._player.package.engine.adproxy.engine.movieinfo.current.originalData.data.program.video.find(a=>a._selected);
>     var m3u8Content = info.m3u8;
>     var title = document.querySelector('.intl-album-title span span').innerText.trim() + "_" + document.querySelector('.intl-play-title a').nextSibling.nodeValue + "_" + info.scrsz + "_" + (info.code == 2 ? "H264": "H265") + "_" + document.getElementsByClassName("iqp-time-dur")[0].innerText.replace(/:/, ".") + "_" + (info.vsize / 1024 / 1024).toFixed(2) + "MB.m3u8";
>     if(m3u8Content!="") download(m3u8Content, title);
> };
> ndIq();
> ```
> 
> ## 爱奇艺(iqiyi.com)音轨下载
> 
> ```js
> var download = function (text, filename) {
>     let b = new Blob([text], {
>       type: 'text/plain'
>     });
>     let a = document.createElement("a");
>     a.href = URL.createObjectURL(b);
>     a.setAttribute("download", filename);
>     a.click();
> };
> var importCmd5xAsync = async function() {
>     let resp = await fetch("https://static.iqiyi.com/js/common/f6a3054843de4645b34d205a9f377d25.js");
>     let jsCode = await resp.text();
>     let script = document.createElement("script");
>     script.text = jsCode;
>     document.getElementsByTagName("head")[0].appendChild(script);
> };
> var getCookie = function (sName) {
>     var aCookie = document.cookie.split("; ");
>     for (var i = 0; i < aCookie.length; i++) {
>         var aCrumb = aCookie[i].split("=");
>         if (sName == aCrumb[0])
>             return unescape(aCrumb[1]);
>     }
>     return null;
> };
> var doRequestAsync = async function(lid, ct, cf) {
>     let movieinfo = playerObject._player.package.engine.adproxy.engine.movieinfo;
>     let src = "01010031010000000000";
>     let host = "https://cache.video.iqiyi.com";
>     let params = "/dash?tvid=" + movieinfo.tvid
>         + "&bid=300&vid=" + movieinfo.vid
>         + "&src=" + src + "&vt=0&rs=1&uid=" + getCookie("P00003")
>         + "&ori=pcw&ps=0&k_uid=" + getCookie("QC005")
>         + "&pt=0&d=0&s=&lid=" + lid + "&ct=" + ct + "&cf=" + (cf == "aac" ? "2" : "1") + "&k_tag=1&ost=0&ppt=0&dfp=" + getCookie("__dfp")
>         + "&locale=zh_cn&k_err_retries=0&qd_v=2&tm=" + (new Date()).getTime()
>         + "&qdy=a&qds=0&k_ft1=740531601218477&k_ft4=1162183859249156&k_ft5=1&ut=1&ut=5";
>     let api = host + params + "&vf=" + cmd5x(params);
>     let resp = await fetch(api,{credentials: 'include'});
>     return await resp.json();
> };
> var ndIqyAsync = async function() {
>     if (audioTracks) audioTracks = undefined;
>     var audioTracks = [];
>     await importCmd5xAsync();
>     var json = await doRequestAsync();
>     var info = json.data.program.audio;
>     info.forEach(function (item, index) {
>         let _track = {};
>         _track.bid = item.bid;
>         _track.name = item.name;
>         _track.cf = item.cf;
>         _track.ct = item.ct;
>         _track.lid = item.lid;
>         audioTracks.push(_track);
>     });
>     audioTracks.sort((a1, a2) => {
>         return (a2.name < a1.name ? 1 : (a2.name == a1.name ? 0 : -1)) + (a2.bid - a1.bid);
>     });
>     let p = "";
>     audioTracks.forEach(function (item, index) {
>         p += `\r\n[${index}]: {${item.name || ""}_${item.bid}_${item.cf}}`;
>     });
>     let _input = prompt("请选择音轨" + p);
>     if (!_input) return;
>     let _select = Number(_input);
>     let _lid = audioTracks[_select].lid;
>     let _ct = audioTracks[_select].ct;
>     let _cf = audioTracks[_select].cf;
>     var json = await doRequestAsync(_lid, _ct, _cf);
>     var info = json.data.program.audio.find(a=>a._selected);
>     var aSize = 0;
>     var fs = info.fs;
>     var content = "#EXTM3U\n";
>     var prefix = "https://data.video.iqiyi.com/videos";
>     let results = fs.map(async fs_i => {
>         let url = fs_i.l;
>         aSize += fs_i.b;
>         let api = prefix + url;
>         let t = "";
>         if(json.data.boss_ts){
>             t = json.data.boss_ts.data.t;
>         }
>         let vid = playerObject._player.package.engine.adproxy.engine.movieinfo.vid;
>         let su = getCookie("QC005");
>         api = prefix + url + "&t=" + t + "&vid=" + vid
>                 + "&QY00001=" + /qd_uid=(\d+)/g.exec(url)[1] + "&su="
>                 + su + "&ib=4&ibt=" + cmd5x(t + /\/(\w{10,})/g.exec(url)[1]) + "&ptime=0";
>         let resp = await fetch(api);
>         return "#EXTINF:"+(fs_i.d/1000).toFixed(2)+"\n" + (await resp.json())["l"];
>     });
>     let urls = await Promise.all(results);
>     content += urls.join('\n');
>     content += "#EXT-X-ENDLIST";
>     var title = document.querySelector('h1.player-title a.title-link[title]')['title'] + "_" + document.querySelector('h1.player-title em').innerText + `_${(info.name || "")}_${info.cf}_` + (aSize / 1024 / 1024).toFixed(2) + "MB.m3u8";
>     download(content, title);
> };
> ndIqyAsync();
> ```

