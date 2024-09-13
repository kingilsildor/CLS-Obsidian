<%*
const D = new Date();
const day = D.getDate();
const month = D.getMonth() + 1;
const date = [day, month];  

// Define the blocks as an array of objects with start and end dates
const blocks = [
    { name: "1", start: [2, 9], end: [28, 10] },
    { name: "2", start: [28, 10], end: [6, 1] },
    { name: "3", start: [6, 1], end: [3, 2] },
    { name: "4", start: [3, 2], end: [31, 3] },
    { name: "5", start: [31, 3], end: [2, 6] },
    { name: "6", start: [2, 6], end: [2, 9] },  
    // Looping back to the start of the academic year
];  

// Function to check if current date falls between two dates
function isInBlock(start, end) {
    const [startDay, startMonth] = start;
    const [endDay, endMonth] = end;  

    if (month > startMonth && month < endMonth) {
        return true;
    }
    if (month === startMonth && day >= startDay) {
        return true;
    }
    if (month === endMonth && day <= endDay) {
        return true;
    }
    return false;
}  

// Loop through blocks to find the current block
let current_block;
for (const block of blocks) {
    if (isInBlock(block.start, block.end)) {
        current_block = block.name;
        break;
    }
}
%>
---
block: <% current_block %>
---
