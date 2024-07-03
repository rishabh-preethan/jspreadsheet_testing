<script>
    import { onMount } from 'svelte';
    import jSpreadsheet from 'jspreadsheet-ce';
    import 'jspreadsheet-ce/dist/jspreadsheet.css';
  
    let spreadsheet;
    let instance;
  
    onMount(() => {
      instance = jSpreadsheet(spreadsheet, {
        data: [
          ['=A1+B1', 10],
          [5, 20],
        ],
        columns: [
          { type: 'text', title: 'Column 1', width: 120 },
          { type: 'number', title: 'Column 2', width: 120 },
        ],
      });
    });
  
    function downloadSpreadsheet() {
      const data = instance.getData();
      const csv = jSpreadsheet.helpers.createCSV(instance, { delimiter: ',' });
      const blob = new Blob([csv], { type: 'text/csv' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = 'spreadsheet.csv';
      a.click();
      URL.revokeObjectURL(url);
    }
  </script>
  
  <div bind:this={spreadsheet}></div>
  <button on:click={downloadSpreadsheet}>Download</button>
  
  <style>
    :global(.jexcel) {
      font-family: Arial, sans-serif;
      margin-bottom: 20px;
    }
  </style>
  