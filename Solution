const fs = require('fs');
const readline = require('readline');

function calculateBalanceSheet(inputFile) {
  const data = JSON.parse(fs.readFileSync(inputFile));

  const startDates = [
    data.revenueData[0].startDate,
    data.expenseData[0].startDate
  ];
  const startDate = new Date(Math.min(...startDates));

  const balanceSheet = {};

  data.revenueData.forEach(entry => {
    const date = new Date(entry.startDate);
    const month = `${date.getFullYear()}-${String(date.getMonth() + 1).padStart(2, '0')}`;
    const amount = entry.amount || 0;
    balanceSheet[month] = (balanceSheet[month] || 0) + amount;
  });

  data.expenseData.forEach(entry => {
    const date = new Date(entry.startDate);
    const month = `${date.getFullYear()}-${String(date.getMonth() + 1).padStart(2, '0')}`;
    const amount = entry.amount || 0;
    balanceSheet[month] = (balanceSheet[month] || 0) - amount;
  });

  const sortedBalanceSheet = Object.entries(balanceSheet).sort((a, b) => {
    const [aYear, aMonth] = a[0].split('-').map(Number);
    const [bYear, bMonth] = b[0].split('-').map(Number);
    if (aYear !== bYear) {
      return aYear - bYear;
    } else {
      return aMonth - bMonth;
    }
  });

  return sortedBalanceSheet;
}

// Create a readline interface to read user input
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});

// Prompt the user to enter the input file path
rl.question('Enter the input file path: ', inputFile => {
  // Call the balance sheet calculation function
  const balanceSheet = calculateBalanceSheet(inputFile);

  console.log('Balance sheet:');
  balanceSheet.forEach(([month, balance]) => {
    console.log(`${month}: ${balance}`);
  });

  // Close the readline interface
  rl.close();
});