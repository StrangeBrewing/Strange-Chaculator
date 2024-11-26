import React, { useState, useEffect } from 'react';
import { Card, CardHeader, CardTitle, CardContent } from '@/components/ui/card';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';
import { BarChart, Bar, XAxis, YAxis, CartesianGrid, Tooltip, Legend } from 'recharts';

const formatARS = (value) => {
  return new Intl.NumberFormat('es-AR', {
    style: 'currency',
    currency: 'ARS',
    minimumFractionDigits: 0,
    maximumFractionDigits: 0,
  }).format(value);
};

const FinancialCalculator = () => {
  // Base inputs
  const [baseRevenue, setBaseRevenue] = useState(53400000);
  const [additionalDays, setAdditionalDays] = useState(1);
  const [newDayRevenue, setNewDayRevenue] = useState(1800000);
  const [newEmployees, setNewEmployees] = useState(2);
  const [employeeCost, setEmployeeCost] = useState(1500000);
  const [priceIncrease, setPriceIncrease] = useState(5);
  
  // Current costs
  const [baseCogsPercent, setBaseCogsPercent] = useState(21.2);
  const [taxRate, setTaxRate] = useState(19.7);
  const [indirectCostPercent, setIndirectCostPercent] = useState(10);
  const [electricityCost, setElectricityCost] = useState(1500000);
  const [waterCost, setWaterCost] = useState(222000);

  // Calculated results
  const [results, setResults] = useState({
    fiveDay: { revenue: 0, costs: 0, profit: 0 },
    sixDay: { revenue: 0, costs: 0, profit: 0 },
    sevenDay: { revenue: 0, costs: 0, profit: 0 }
  });

  const calculateScenarios = () => {
    // Adjust COGS for price increase
    const adjustedCogsPercent = baseCogsPercent / (1 + (priceIncrease / 100));
    
    // 5-day scenario with price increase
    const fiveDay = {
      revenue: baseRevenue * (1 + (priceIncrease / 100)),
      costs: baseRevenue * (adjustedCogsPercent / 100) +
             baseRevenue * (taxRate / 100) +
             baseRevenue * (indirectCostPercent / 100) +
             electricityCost + waterCost,
    };
    fiveDay.profit = fiveDay.revenue - fiveDay.costs;

    // 6-day scenario
    const sixDayAddedRevenue = newDayRevenue * 4 * (1 + (priceIncrease / 100));
    const sixDay = {
      revenue: fiveDay.revenue + sixDayAddedRevenue,
      costs: (fiveDay.revenue + sixDayAddedRevenue) * (adjustedCogsPercent / 100) +
             (fiveDay.revenue + sixDayAddedRevenue) * (taxRate / 100) +
             (fiveDay.revenue + sixDayAddedRevenue) * (indirectCostPercent / 100) +
             (electricityCost + waterCost) * 1.2 +
             (additionalDays === 1 ? newEmployees * employeeCost : 0),
    };
    sixDay.profit = sixDay.revenue - sixDay.costs;

    // 7-day scenario
    const sevenDayAddedRevenue = newDayRevenue * 8 * (1 + (priceIncrease / 100));
    const sevenDay = {
      revenue: fiveDay.revenue + sevenDayAddedRevenue,
      costs: (fiveDay.revenue + sevenDayAddedRevenue) * (adjustedCogsPercent / 100) +
             (fiveDay.revenue + sevenDayAddedRevenue) * (taxRate / 100) +
             (fiveDay.revenue + sevenDayAddedRevenue) * (indirectCostPercent / 100) +
             (electricityCost + waterCost) * 1.4 +
             (additionalDays === 2 ? newEmployees * employeeCost : 0),
    };
    sevenDay.profit = sevenDay.revenue - sevenDay.costs;

    setResults({ fiveDay, sixDay, sevenDay });
  };

  useEffect(() => {
    calculateScenarios();
  }, [baseRevenue, additionalDays, newDayRevenue, newEmployees, employeeCost, 
      priceIncrease, baseCogsPercent, taxRate, indirectCostPercent, 
      electricityCost, waterCost]);

  const chartData = [
    {
      name: '5 Days',
      Revenue: results.fiveDay.revenue,
      Costs: results.fiveDay.costs,
      'Net Profit': results.fiveDay.profit
    },
    {
      name: '6 Days',
      Revenue: results.sixDay.revenue,
      Costs: results.sixDay.costs,
      'Net Profit': results.sixDay.profit
    },
    {
      name: '7 Days',
      Revenue: results.sevenDay.revenue,
      Costs: results.sevenDay.costs,
      'Net Profit': results.sevenDay.profit
    }
  ];

  return (
    <div className="w-full max-w-6xl mx-auto p-4 space-y-6">
      <Card>
        <CardHeader>
          <CardTitle>Strange Brewing Financial Calculator</CardTitle>
        </CardHeader>
        <CardContent>
          <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            {/* Base Inputs */}
            <div className="space-y-4">
              <h3 className="font-semibold">Base Parameters</h3>
              <div>
                <Label>Current Monthly Revenue (ARS)</Label>
                <Input 
                  type="number" 
                  value={baseRevenue} 
                  onChange={(e) => setBaseRevenue(Number(e.target.value))}
                />
              </div>
              <div>
                <Label>New Day Revenue (ARS)</Label>
                <Input 
                  type="number" 
                  value={newDayRevenue} 
                  onChange={(e) => setNewDayRevenue(Number(e.target.value))}
                />
              </div>
              <div>
                <Label>Price Increase (%)</Label>
                <Input 
                  type="number" 
                  value={priceIncrease} 
                  onChange={(e) => setPriceIncrease(Number(e.target.value))}
                />
              </div>
            </div>

            {/* Cost Inputs */}
            <div className="space-y-4">
              <h3 className="font-semibold">Cost Parameters</h3>
              <div>
                <Label>COGS (%)</Label>
                <Input 
                  type="number" 
                  value={baseCogsPercent} 
                  onChange={(e) => setBaseCogsPercent(Number(e.target.value))}
                />
              </div>
              <div>
                <Label>New Employees</Label>
                <Input 
                  type="number" 
                  value={newEmployees} 
                  onChange={(e) => setNewEmployees(Number(e.target.value))}
                />
              </div>
              <div>
                <Label>Cost per Employee (ARS)</Label>
                <Input 
                  type="number" 
                  value={employeeCost} 
                  onChange={(e) => setEmployeeCost(Number(e.target.value))}
                />
              </div>
            </div>

            {/* Results Summary */}
            <div className="space-y-4">
              <h3 className="font-semibold">Monthly Results</h3>
              <div className="space-y-2">
                <p>5 Days: {formatARS(results.fiveDay.profit)}</p>
                <p>6 Days: {formatARS(results.sixDay.profit)}</p>
                <p>7 Days: {formatARS(results.sevenDay.profit)}</p>
              </div>
            </div>
          </div>

          {/* Chart */}
          <div className="mt-8">
            <BarChart width={800} height={400} data={chartData}>
              <CartesianGrid strokeDasharray="3 3" />
              <XAxis dataKey="name" />
              <YAxis />
              <Tooltip formatter={(value) => formatARS(value)} />
              <Legend />
              <Bar dataKey="Revenue" fill="#4ade80" />
              <Bar dataKey="Costs" fill="#f87171" />
              <Bar dataKey="Net Profit" fill="#60a5fa" />
            </BarChart>
          </div>
        </CardContent>
      </Card>
    </div>
  );
};

export default FinancialCalculator;
