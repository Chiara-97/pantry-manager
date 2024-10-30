import React, { useState } from 'react';
import { Plus, Search, AlertCircle, Star, Camera, Minus } from 'lucide-react';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import { Tabs, TabsContent, TabsList, TabsTrigger } from '@/components/ui/tabs';
import { Input } from '@/components/ui/input';
import { Button } from '@/components/ui/button';
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from '@/components/ui/select';
import { Alert, AlertDescription } from '@/components/ui/alert';

const PantryManager = () => {
  const [products, setProducts] = useState([]);
  const [searchTerm, setSearchTerm] = useState('');
  const [customLocation, setCustomLocation] = useState('');
  
  const categories = {
    'food': 'Cibo',
    'cleaning': 'Prodotti per la Pulizia',
    'necessities': 'Necessità Quotidiane',
    'other': 'Altro'
  };
  
  const predefinedLocations = {
    'pantry': 'Dispensa',
    'fridge': 'Frigorifero',
    'freezer': 'Congelatore',
    'cantina': 'Cantina',
    'custom': 'Altra Posizione',
  };

  const addProduct = (product) => {
    setProducts([...products, { 
      ...product, 
      id: Date.now(),
      location: product.location === 'custom' ? customLocation : product.location
    }]);
  };

  const getDaysUntilExpiry = (expiryDate) => {
    const today = new Date();
    const expiry = new Date(expiryDate);
    const diffTime = expiry - today;
    return Math.ceil(diffTime / (1000 * 60 * 60 * 24));
  };

  // Add Product Form Component
  const AddProductForm = () => {
    const [newProduct, setNewProduct] = useState({
      name: '',
      category: '',
      location: '',
      expiryDate: '',
      notes: '',
      rating: 0,
      quantity: 1
    });

    const updateQuantity = (increment) => {
      setNewProduct(prev => ({
        ...prev,
        quantity: Math.max(0, prev.quantity + increment)
      }));
    };

    return (
      <Card className="w-full max-w-2xl mx-auto">
        <CardHeader>
          <CardTitle className="text-2xl">Aggiungi Nuovo Prodotto</CardTitle>
        </CardHeader>
        <CardContent>
          <form className="space-y-4">
            <div>
              <label className="text-lg font-medium">Nome Prodotto</label>
              <Input 
                placeholder="Nome del prodotto"
                className="mt-1"
                value={newProduct.name}
                onChange={(e) => setNewProduct({...newProduct, name: e.target.value})}
              />
            </div>

            <div className="grid grid-cols-2 gap-4">
              <div>
                <label className="text-lg font-medium">Categoria</label>
                <Select onValueChange={(value) => setNewProduct({...newProduct, category: value})}>
                  <SelectTrigger>
                    <SelectValue placeholder="Seleziona categoria" />
                  </SelectTrigger>
                  <SelectContent>
                    {Object.entries(categories).map(([key, value]) => (
                      <SelectItem key={key} value={key}>{value}</SelectItem>
                    ))}
                  </SelectContent>
                </Select>
              </div>

              <div>
                <label className="text-lg font-medium">Posizione</label>
                <Select onValueChange={(value) => {
                  setNewProduct({...newProduct, location: value});
                  if (value !== 'custom') {
                    setCustomLocation('');
                  }
                }}>
                  <SelectTrigger>
                    <SelectValue placeholder="Seleziona posizione" />
                  </SelectTrigger>
                  <SelectContent>
                    {Object.entries(predefinedLocations).map(([key, value]) => (
                      <SelectItem key={key} value={key}>{value}</SelectItem>
                    ))}
                  </SelectContent>
                </Select>
                {newProduct.location === 'custom' && (
                  <Input 
                    placeholder="Specifica la posizione"
                    className="mt-2"
                    value={customLocation}
                    onChange={(e) => setCustomLocation(e.target.value)}
                  />
                )}
              </div>
            </div>

            <div className="grid grid-cols-2 gap-4">
              <div>
                <label className="text-lg font-medium">Data di Scadenza</label>
                <Input 
                  type="date"
                  className="mt-1"
                  value={newProduct.expiryDate}
                  onChange={(e) => setNewProduct({...newProduct, expiryDate: e.target.value})}
                />
              </div>

              <div>
                <label className="text-lg font-medium">Quantità</label>
                <div className="flex items-center gap-2 mt-1">
                  <Button 
                    type="button"
                    variant="outline"
                    size="icon"
                    onClick={() => updateQuantity(-1)}
                  >
                    <Minus className="h-4 w-4" />
                  </Button>
                  <Input 
                    type="number"
                    value={newProduct.quantity}
                    onChange={(e) => setNewProduct({...newProduct, quantity: parseInt(e.target.value) || 0})}
                    className="w-20 text-center"
                  />
                  <Button 
                    type="button"
                    variant="outline"
                    size="icon"
                    onClick={() => updateQuantity(1)}
                  >
                    <Plus className="h-4 w-4" />
                  </Button>
                </div>
              </div>
            </div>

            <div>
              <label className="text-lg font-medium">Note</label>
              <Input 
                placeholder="Aggiungi note"
                className="mt-1"
                value={newProduct.notes}
                onChange={(e) => setNewProduct({...newProduct, notes: e.target.value})}
              />
            </div>

            <div>
              <label className="text-lg font-medium">Valutazione</label>
              <div className="flex gap-1 mt-1">
                {[1, 2, 3, 4, 5].map((rating) => (
                  <Star
                    key={rating}
                    className={`cursor-pointer ${rating <= newProduct.rating ? 'fill-yellow-400 text-yellow-400' : 'text-gray-300'}`}
                    onClick={() => setNewProduct({...newProduct, rating})}
                  />
                ))}
              </div>
            </div>

            <div className="flex justify-between gap-4">
              <Button 
                type="button"
                className="flex-1"
                onClick={() => document.getElementById('photo-upload').click()}
              >
                <Camera className="w-4 h-4 mr-2" />
                Carica Foto
              </Button>
              <input
                id="photo-upload"
                type="file"
                accept="image/*"
                className="hidden"
                onChange={(e) => {
                  // Handle photo upload
                }}
              />
              <Button 
                type="submit"
                className="flex-1"
                onClick={(e) => {
                  e.preventDefault();
                  addProduct(newProduct);
                  setNewProduct({
                    name: '',
                    category: '',
                    location: '',
                    expiryDate: '',
                    notes: '',
                    rating: 0,
                    quantity: 1
                  });
                  setCustomLocation('');
                }}
              >
                <Plus className="w-4 h-4 mr-2" />
                Aggiungi
              </Button>
            </div>
          </form>
        </CardContent>
      </Card>
    );
  };

  // Product List Component
  const ProductList = () => {
    const filteredProducts = products.filter(product => 
      product.name.toLowerCase().includes(searchTerm.toLowerCase()) ||
      product.category.toLowerCase().includes(searchTerm.toLowerCase()) ||
      product.notes.toLowerCase().includes(searchTerm.toLowerCase())
    );

    const groupedProducts = filteredProducts.reduce((acc, product) => {
      if (!acc[product.category]) {
        acc[product.category] = [];
      }
      acc[product.category].push(product);
      return acc;
    }, {});

    Object.values(groupedProducts).forEach(group => {
      group.sort((a, b) => a.name.localeCompare(b.name));
    });

    return (
      <Card className="w-full max-w-4xl mx-auto">
        <CardHeader>
          <CardTitle className="text-2xl">Lista Prodotti</CardTitle>
          <div className="relative">
            <Search className="absolute left-2 top-3 h-4 w-4 text-gray-500" />
            <Input
              placeholder="Cerca prodotti..."
              className="pl-8"
              value={searchTerm}
              onChange={(e) => setSearchTerm(e.target.value)}
            />
          </div>
        </CardHeader>
        <CardContent>
          {Object.entries(groupedProducts).map(([category, items]) => (
            <div key={category} className="mb-6">
              <h3 className="text-xl font-semibold mb-3">{categories[category]}</h3>
              <div className="space-y-2">
                {items.map(product => (
                  <div key={product.id} className="flex items-center justify-between p-2 bg-gray-50 rounded">
                    <div>
                      <span className="font-medium">{product.name}</span>
                      <div className="text-sm text-gray-600">
                        {product.location === 'custom' ? customLocation : predefinedLocations[product.location]} • 
                        Scade: {product.expiryDate} • 
                        Quantità: {product.quantity}
                      </div>
                    </div>
                    <div className="flex gap-2">
                      {[1, 2, 3, 4, 5].map((star) => (
                        <Star
                          key={star}
                          className={`w-4 h-4 ${star <= product.rating ? 'fill-yellow-400 text-yellow-400' : 'text-gray-300'}`}
                        />
                      ))}
                    </div>
                  </div>
                ))}
              </div>
            </div>
          ))}
        </CardContent>
      </Card>
    );
  };

  // Expiry Alerts Component
  const ExpiryAlerts = () => {
    const sortedProducts = [...products].sort((a, b) => {
      const daysA = getDaysUntilExpiry(a.expiryDate);
      const daysB = getDaysUntilExpiry(b.expiryDate);
      return daysA - daysB;
    });

    return (
      <Card className="w-full max-w-2xl mx-auto">
        <CardHeader>
          <CardTitle className="text-2xl">Avvisi Scadenza</CardTitle>
        </CardHeader>
        <CardContent className="space-y-4">
          {sortedProducts.map(product => {
            const daysUntilExpiry = getDaysUntilExpiry(product.expiryDate);
            const isLowStock = product.quantity <= 1;
            
            let bgColor = 'bg-white';
            if (daysUntilExpiry <= 3) bgColor = 'bg-red-100';
            else if (daysUntilExpiry <= 7) bgColor = 'bg-orange-100';

            return (
              <Alert 
                key={product.id} 
                className={`flex items-center justify-between ${bgColor}`}
              >
                <div className="flex items-center">
                  <AlertCircle className="h-4 w-4" />
                  <AlertDescription className="ml-2">
                    <span className="font-medium">{product.name}</span>
                    {daysUntilExpiry <= 0 ? (
                      <span className="text-red-600"> - SCADUTO!</span>
                    ) : (
                      <span> - Scade tra {daysUntilExpiry} giorni</span>
                    )}
                  </AlertDescription>
                </div>
                {product.quantity === 0 ? (
                  <span className="text-red-600 font-medium">Esaurito</span>
                ) : isLowStock && (
                  <span className="text-orange-600 font-medium">Scorta bassa: {product.quantity}</span>
                )}
              </Alert>
            );
          })}
        </CardContent>
      </Card>
    );
  };

  return (
    <div className="min-h-screen bg-gray-50 p-4">
      <div className="max-w-6xl mx-auto">
        <h1 className="text-3xl font-bold text-center mb-8">Gestione Dispensa</h1>
        
        <Tabs defaultValue="add" className="space-y-4">
          <TabsList className="grid w-full grid-cols-3">
            <TabsTrigger value="add">Aggiungi Prodotto</TabsTrigger>
            <TabsTrigger value="list">Lista Prodotti</TabsTrigger>
            <TabsTrigger value="alerts">Avvisi</TabsTrigger>
          </TabsList>

          <TabsContent value="add">
            <AddProductForm />
          </TabsContent>

          <TabsContent value="list">
            <ProductList />
          </TabsContent>

          <TabsContent value="alerts">
            <ExpiryAlerts />
          </TabsContent>
        </Tabs>
      </div>
    </div>
  );
};

export default PantryManager;
