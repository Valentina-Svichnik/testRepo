```
.dark .markdown-container h6 {
    color: #9D9D9D;
}

.markdown-container h1 {
    font-size: 2em;
    font-weight: 600;
    padding-bottom: 0.3em;
    border-bottom: 1px solid #EBEBEB;
    margin-bottom: 16px;
    line-height: 1.25;
}

.markdown-container h2 {
    font-size: 1.5em;
    font-weight: 600;
    padding-bottom: 0.2em;
    border-bottom: 1px solid #EBEBEB;
    margin-bottom: 10px;
    line-height: 1;
}

.markdown-container h3 {
    font-size: 1.2em;
    font-weight: 600;
    padding-bottom: 0.25em;
    margin-bottom: 10px;
    line-height: 1;
}

.markdown-container h4 {
    font-size: 1em;
    font-weight: 600;
    padding-bottom: 0.2em;
    margin-bottom: 10px;
    line-height: 1;
}

.markdown-container h5 {
    font-size: 0.9em;
    font-weight: 600;
    padding-bottom: 0.2em;
    margin-bottom: 10px;
    line-height: 1;
}

.markdown-container h6 {
    font-size: 0.9em;
    font-weight: 600;
    padding-bottom: 0.2em;
    margin-bottom: 10px;
    line-height: 1;
    color: #606060;
}

.markdown-container p {
    margin-bottom: 5px;
}

.markdown-container hr{
    margin: 30px 0;
}

.markdown-container blockquote{
    padding-left: 50px;
    border-left: 4px solid #e2e2e2;
    font-family: 'Georgia', 'Times New Roman', serif;
}

.markdown-container blockquote p {
    color: #333333;
}

.dark .markdown-container blockquote p {
    color: #929292;
}

.markdown-container blockquote cite {
    font-style: italic;
    color: #aaaaaa;
}

.markdown-container ol {
	counter-reset: num;
	margin: 0 0 0 35px;
	padding: 15px 0 5px 0;
}
.markdown-container ol li {
	position: relative;	
	margin: 0 0 0 0;
	padding: 0 0 10px 0;
}

.markdown-container ol li:before {
	content: counter(num) '.'; 
	counter-increment: num;
	display: inline-block;	
	position: absolute;
	top: 0px;
	left: -26px;
	width: 20px;    
	text-align: right;
}

.markdown-container ul {
	list-style-type: disc;
    margin: 0 0 0 35px;
}

.markdown-container a{
    color: #497DDD; 
    border-bottom: .07em solid;
}

.markdown-container a:hover{
    color: #0039A1; 
}

.markdown-container pre{
    padding: 10px 20px;
    background-color: #F6F8FA;
    margin: 5px 0;
    border-radius: 5px;
}

.dark .markdown-container pre{
    background-color: #1B1918;
}

.dark .markdown-container code{
    background-color: #1B1918;
}

.markdown-container code{
    padding: 3px;
    background-color: #F6F8FA;
    border-radius: 3px;
}

.markdown-container table {
	width: 100%;
	margin-bottom: 20px;
	border: 1px solid #dddddd;
	border-collapse: collapse; 
    margin-top: 20px;
}


.dark .markdown-container table th, .dark .markdown-container table td {
	border: 1px solid #343E4B;
}

.markdown-container table th {
	font-weight: bold;
	padding: 5px;
	background: #F6F8FA;
	border: 1px solid #dddddd;
    text-align: start;
}

.dark .markdown-container table th {
	background: #1B1918;
}

.markdown-container table td {
	border: 1px solid #dddddd;
	padding: 5px;
    text-align: start;
}