<%*
let strtitle = await tp.system.prompt(“Task Line Summary:”)
let actualDate = await tp.date.now(‘YYYY-MM-DD’);
tp.file.create_new(actualdate+“-”+strtitle)
tR +=  “[” + “[” + actual data + "-" + strtitle + “]]”;
%>