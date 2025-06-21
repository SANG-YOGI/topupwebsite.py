import { useState } from 'react'
import { Button } from "/components/ui/button"
import { Card, CardContent, CardHeader, CardTitle, CardDescription } from "/components/ui/card"
import { Input } from "/components/ui/input"
import { Label } from "/components/ui/label"
import { RadioGroup, RadioGroupItem } from "/components/ui/radio-group"

type DiamondPackage = {
  id: string
  diamonds: number
  price: number
  bonus?: number
}

type MembershipOption = {
  id: string
  name: string
  duration: string
  price: number
  benefits: string[]
}

export default function FreeFireTopUp() {
  const [currentStep, setCurrentStep] = useState<'selection' | 'details' | 'payment' | 'confirmation' | 'complete'>('selection')
  const [selectedTab, setSelectedTab] = useState<'diamonds' | 'membership'>('diamonds')
  const [selectedPackages, setSelectedPackages] = useState<string[]>([])
  const [accountId, setAccountId] = useState('')
  const [paymentMethod, setPaymentMethod] = useState<'bank' | 'card'>('bank')
  const [cardDetails, setCardDetails] = useState({
    number: '',
    name: '',
    expiry: '',
    cvv: ''
  })
  const [bankDetails, setBankDetails] = useState({
    accountNumber: '',
    accountName: ''
  })
  const [transactionId, setTransactionId] = useState('')

  const diamondPackages: DiamondPackage[] = [
    { id: 'd1', diamonds: 25, price: 30 },
    { id: 'd2', diamonds: 50, price: 55 },
    { id: 'd3', diamonds: 115, price: 100 },
    { id: 'd4', diamonds: 240, price: 200 },
    { id: 'd5', diamonds: 355, price: 300 },
    { id: 'd6', diamonds: 480, price: 400 },
    { id: 'd7', diamonds: 610, price: 500 },
    { id: 'd8', diamonds: 725, price: 700 },
    { id: 'd9', diamonds: 850, price: 800 },
    { id: 'd10', diamonds: 1240, price: 1000 },
    { id: 'd11', diamonds: 1850, price: 1600 },
    { id: 'd12', diamonds: 2530, price: 2000 },
    { id: 'd13', diamonds: 3770, price: 3000 },
    { id: 'd14', diamonds: 5060, price: 4400 }
  ]

  const membershipOptions: MembershipOption[] = [
    {
      id: 'm1',
      name: 'Weekly Lite',
      duration: '7 days',
      price: 100,
      benefits: ['Daily rewards', 'Exclusive items']
    },
    {
      id: 'm2',
      name: 'Weekly',
      duration: '7 days',
      price: 200,
      benefits: ['More rewards than Lite', 'Premium items']
    },
    {
      id: 'm3',
      name: 'Monthly',
      duration: '30 days',
      price: 1000,
      benefits: ['Best value', 'All premium items', 'VIP status']
    },
    {
      id: 'm4',
      name: 'Level Up Pass',
      duration: 'Seasonal',
      price: 200,
      benefits: ['Exclusive rewards', 'Level up faster']
    },
    {
      id: 'm5',
      name: 'Super VIP',
      duration: '30 days',
      price: 1200,
      benefits: ['All VIP benefits', 'Exclusive skins', 'Priority support']
    }
  ]

  const togglePackageSelection = (id: string) => {
    if (selectedPackages.includes(id)) {
      setSelectedPackages(selectedPackages.filter(pkg => pkg !== id))
    } else {
      setSelectedPackages([...selectedPackages, id])
    }
  }

  const calculateTotal = () => {
    let totalPrice = 0
    let totalDiamonds = 0

    if (selectedTab === 'diamonds') {
      selectedPackages.forEach(id => {
        const pkg = diamondPackages.find(p => p.id === id)
        if (pkg) {
          totalPrice += pkg.price
          totalDiamonds += pkg.diamonds + (pkg.bonus || 0)
        }
      })
    } else {
      selectedPackages.forEach(id => {
        const membership = membershipOptions.find(m => m.id === id)
        if (membership) {
          totalPrice += membership.price
        }
      })
    }

    return { totalPrice, totalDiamonds }
  }

  const handlePaymentSubmit = () => {
    // In a real app, this would call your payment API
    // For demo, we'll just generate a random transaction ID
    setTransactionId(`FF-${Math.floor(Math.random() * 1000000)}`)
    setCurrentStep('complete')
  }

  const resetPurchase = () => {
    setSelectedPackages([])
    setAccountId('')
    setCurrentStep('selection')
  }

  const { totalPrice, totalDiamonds } = calculateTotal()

  return (
    <div className="min-h-screen bg-gray-50 py-8 px-4">
      <div className="max-w-3xl mx-auto">
        <Card>
          <CardHeader>
            <CardTitle className="text-2xl">FreeFire Top-Up</CardTitle>
            <CardDescription>
              {currentStep === 'selection' && 'Select diamond packages or membership options'}
              {currentStep === 'details' && 'Enter your FreeFire account details'}
              {currentStep === 'payment' && 'Select payment method'}
              {currentStep === 'confirmation' && 'Confirm your purchase'}
              {currentStep === 'complete' && 'Purchase complete!'}
            </CardDescription>
          </CardHeader>

          <CardContent>
            {/* Step indicator */}
            <div className="flex justify-between mb-8">
              <div className={`flex items-center ${currentStep === 'selection' ? 'text-primary font-bold' : 'text-gray-500'}`}>
                <div className={`w-8 h-8 rounded-full flex items-center justify-center ${currentStep === 'selection' ? 'bg-primary text-white' : 'bg-gray-200'}`}>1</div>
                <span className="ml-2">Selection</span>
              </div>
              <div className={`flex items-center ${currentStep === 'details' ? 'text-primary font-bold' : currentStep === 'payment' || currentStep === 'confirmation' || currentStep === 'complete' ? 'text-gray-500' : 'text-gray-300'}`}>
                <div className={`w-8 h-8 rounded-full flex items-center justify-center ${currentStep === 'details' ? 'bg-primary text-white' : currentStep === 'payment' || currentStep === 'confirmation' || currentStep === 'complete' ? 'bg-gray-200' : 'bg-gray-100'}`}>2</div>
                <span className="ml-2">Details</span>
              </div>
              <div className={`flex items-center ${currentStep === 'payment' ? 'text-primary font-bold' : currentStep === 'confirmation' || currentStep === 'complete' ? 'text-gray-500' : 'text-gray-300'}`}>
                <div className={`w-8 h-8 rounded-full flex items-center justify-center ${currentStep === 'payment' ? 'bg-primary text-white' : currentStep === 'confirmation' || currentStep === 'complete' ? 'bg-gray-200' : 'bg-gray-100'}`}>3</div>
                <span className="ml-2">Payment</span>
              </div>
              <div className={`flex items-center ${currentStep === 'confirmation' ? 'text-primary font-bold' : currentStep === 'complete' ? 'text-gray-500' : 'text-gray-300'}`}>
                <div className={`w-8 h-8 rounded-full flex items-center justify-center ${currentStep === 'confirmation' ? 'bg-primary text-white' : currentStep === 'complete' ? 'bg-gray-200' : 'bg-gray-100'}`}>4</div>
                <span className="ml-2">Confirm</span>
              </div>
            </div>

            {/* Selection Step */}
            {currentStep === 'selection' && (
              <div>
                <div className="flex border-b mb-6">
                  <button
                    className={`px-4 py-2 font-medium ${selectedTab === 'diamonds' ? 'border-b-2 border-primary text-primary' : 'text-gray-500'}`}
                    onClick={() => setSelectedTab('diamonds')}
                  >
                    Diamond Packages
                  </button>
                  <button
                    className={`px-4 py-2 font-medium ${selectedTab === 'membership' ? 'border-b-2 border-primary text-primary' : 'text-gray-500'}`}
                    onClick={() => setSelectedTab('membership')}
                  >
                    Membership
                  </button>
                </div>

                {selectedTab === 'diamonds' ? (
                  <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                    {diamondPackages.map(pkg => (
                      <Card
                        key={pkg.id}
                        className={`cursor-pointer ${selectedPackages.includes(pkg.id) ? 'border-primary bg-primary/10' : ''}`}
                        onClick={() => togglePackageSelection(pkg.id)}
                      >
                        <CardContent className="p-4">
                          <div className="flex justify-between items-center">
                            <div>
                              <h3 className="font-bold text-lg">{pkg.diamonds} Diamonds</h3>
                              {pkg.bonus && <p className="text-sm text-green-600">+ {pkg.bonus} Bonus</p>}
                            </div>
                            <div className="text-right">
                              <p className="font-bold">Rs. {pkg.price}</p>
                            </div>
                          </div>
                        </CardContent>
                      </Card>
                    ))}
                  </div>
                ) : (
                  <div className="grid grid-cols-1 gap-4">
                    {membershipOptions.map(membership => (
                      <Card
                        key={membership.id}
                        className={`cursor-pointer ${selectedPackages.includes(membership.id) ? 'border-primary bg-primary/10' : ''}`}
                        onClick={() => togglePackageSelection(membership.id)}
                      >
                        <CardContent className="p-4">
                          <div className="flex justify-between items-start">
                            <div>
                              <h3 className="font-bold text-lg">{membership.name}</h3>
                              <p className="text-sm text-gray-600">{membership.duration}</p>
                              <ul className="mt-2 text-sm list-disc pl-5">
                                {membership.benefits.map((benefit, i) => (
                                  <li key={i}>{benefit}</li>
                                ))}
                              </ul>
                            </div>
                            <div className="text-right">
                              <p className="font-bold">Rs. {membership.price}</p>
                            </div>
                          </div>
                        </CardContent>
                      </Card>
                    ))}
                  </div>
                )}

                {selectedPackages.length > 0 && (
                  <div className="mt-6 p-4 bg-gray-100 rounded-lg">
                    <h3 className="font-bold mb-2">Selected Items</h3>
                    {selectedTab === 'diamonds' ? (
                      <div>
                        {selectedPackages.map(id => {
                          const pkg = diamondPackages.find(p => p.id === id)
                          return pkg ? (
                            <div key={id} className="flex justify-between py-1">
                              <span>{pkg.diamonds} Diamonds{pkg.bonus ? ` (+${pkg.bonus})` : ''}</span>
                              <span>Rs. {pkg.price}</span>
                            </div>
                          ) : null
                        })}
                        <div className="border-t mt-2 pt-2 font-bold flex justify-between">
                          <span>Total Diamonds:</span>
                          <span>{totalDiamonds}</span>
                        </div>
                      </div>
                    ) : (
                      <div>
                        {selectedPackages.map(id => {
                          const membership = membershipOptions.find(m => m.id === id)
                          return membership ? (
                            <div key={id} className="flex justify-between py-1">
                              <span>{membership.name}</span>
                              <span>Rs. {membership.price}</span>
                            </div>
                          ) : null
                        })}
                      </div>
                    )}
                    <div className="border-t mt-2 pt-2 font-bold flex justify-between">
                      <span>Total Price:</span>
                      <span>Rs. {totalPrice}</span>
                    </div>
                    <Button className="w-full mt-4" onClick={() => setCurrentStep('details')}>
                      Continue
                    </Button>
                  </div>
                )}
              </div>
            )}

            {/* Details Step */}
            {currentStep === 'details' && (
              <div>
                <div className="space-y-4">
                  <div>
                    <Label htmlFor="accountId">FreeFire Account ID</Label>
                    <Input
                      id="accountId"
                      placeholder="Enter your FreeFire account ID"
                      value={accountId}
                      onChange={(e) => setAccountId(e.target.value)}
                    />
                    <p className="text-sm text-gray-500 mt-1">This is usually your in-game player ID</p>
                  </div>
                </div>
                <div className="flex justify-between mt-6">
                  <Button variant="outline" onClick={() => setCurrentStep('selection')}>
                    Back
                  </Button>
                  <Button onClick={() => setCurrentStep('payment')} disabled={!accountId.trim()}>
                    Continue
                  </Button>
                </div>
              </div>
            )}

            {/* Payment Step */}
            {currentStep === 'payment' && (
              <div>
                <div className="space-y-6">
                  <div>
                    <h3 className="font-bold mb-3">Payment Method</h3>
                    <RadioGroup value={paymentMethod} onValueChange={(value) => setPaymentMethod(value as 'bank' | 'card')}>
                      <div className="flex items-center space-x-2">
                        <RadioGroupItem value="bank" id="bank" />
                        <Label htmlFor="bank">Bank Transfer</Label>
                      </div>
                      <div className="flex items-center space-x-2">
                        <RadioGroupItem value="card" id="card" />
                        <Label htmlFor="card">Credit/Debit Card</Label>
                      </div>
                    </RadioGroup>
                  </div>

                  {paymentMethod === 'bank' && (
                    <div className="space-y-4">
                      <h3 className="font-bold">Bank Details</h3>
                      <div>
                        <Label htmlFor="bankAccountNumber">Account Number</Label>
                        <Input
                          id="bankAccountNumber"
                          placeholder="Enter account number"
                          value={bankDetails.accountNumber}
                          onChange={(e) => setBankDetails({...bankDetails, accountNumber: e.target.value})}
                        />
                      </div>
                      <div>
                        <Label htmlFor="bankAccountName">Account Name</Label>
                        <Input
                          id="bankAccountName"
                          placeholder="Enter account name"
                          value={bankDetails.accountName}
                          onChange={(e) => setBankDetails({...bankDetails, accountName: e.target.value})}
                        />
                      </div>
                    </div>
                  )}

                  {paymentMethod === 'card' && (
                    <div className="space-y-4">
                      <h3 className="font-bold">Card Details</h3>
                      <div>
                        <Label htmlFor="cardNumber">Card Number</Label>
                        <Input
                          id="cardNumber"
                          placeholder="1234 5678 9012 3456"
                          value={cardDetails.number}
                          onChange={(e) => setCardDetails({...cardDetails, number: e.target.value})}
                        />
                      </div>
                      <div>
                        <Label htmlFor="cardName">Cardholder Name</Label>
                        <Input
                          id="cardName"
                          placeholder="Name on card"
                          value={cardDetails.name}
                          onChange={(e) => setCardDetails({...cardDetails, name: e.target.value})}
                        />
                      </div>
                      <div className="grid grid-cols-2 gap-4">
                        <div>
                          <Label htmlFor="cardExpiry">Expiry Date</Label>
                          <Input
                            id="cardExpiry"
                            placeholder="MM/YY"
                            value={cardDetails.expiry}
                            onChange={(e) => setCardDetails({...cardDetails, expiry: e.target.value})}
                          />
                        </div>
                        <div>
                          <Label htmlFor="cardCvv">CVV</Label>
                          <Input
                            id="cardCvv"
                            placeholder="123"
                            value={cardDetails.cvv}
                            onChange={(e) => setCardDetails({...cardDetails, cvv: e.target.value})}
                          />
                        </div>
                      </div>
                    </div>
                  )}
                </div>
                <div className="flex justify-between mt-6">
                  <Button variant="outline" onClick={() => setCurrentStep('details')}>
                    Back
                  </Button>
                  <Button onClick={() => setCurrentStep('confirmation')}>
                    Continue
                  </Button>
                </div>
              </div>
            )}

            {/* Confirmation Step */}
            {currentStep === 'confirmation' && (
              <div>
                <div className="space-y-6">
                  <div>
                    <h3 className="font-bold mb-2">Order Summary</h3>
                    <div className="bg-gray-100 p-4 rounded-lg">
                      <h4 className="font-semibold mb-2">Account Details</h4>
                      <p>FreeFire ID: {accountId}</p>

                      <h4 className="font-semibold mt-4 mb-2">Items</h4>
                      {selectedTab === 'diamonds' ? (
                        <div>
                          {selectedPackages.map(id => {
                            const pkg = diamondPackages.find(p => p.id === id)
                            return pkg ? (
                              <div key={id} className="flex justify-between py-1">
                                <span>{pkg.diamonds} Diamonds{pkg.bonus ? ` (+${pkg.bonus})` : ''}</span>
                                <span>Rs. {pkg.price}</span>
                              </div>
                            ) : null
                          })}
                          <div className="border-t mt-2 pt-2 font-bold flex justify-between">
                            <span>Total Diamonds:</span>
                            <span>{totalDiamonds}</span>
                          </div>
                        </div>
                      ) : (
                        <div>
                          {selectedPackages.map(id => {
                            const membership = membershipOptions.find(m => m.id === id)
                            return membership ? (
                              <div key={id} className="mb-2">
                                <div className="flex justify-between">
                                  <span className="font-medium">{membership.name}</span>
                                  <span>Rs. {membership.price}</span>
                                </div>
                                <p className="text-sm text-gray-600">{membership.duration}</p>
                                <ul className="text-sm list-disc pl-5 mt-1">
                                  {membership.benefits.map((benefit, i) => (
                                    <li key={i}>{benefit}</li>
                                  ))}
                                </ul>
                              </div>
                            ) : null
                          })}
                        </div>
                      )}

                      <div className="border-t mt-2 pt-2 font-bold flex justify-between">
                        <span>Total Price:</span>
                        <span>Rs. {totalPrice}</span>
                      </div>

                      <h4 className="font-semibold mt-4 mb-2">Payment Method</h4>
                      <p className="capitalize">{paymentMethod}</p>
                    </div>
                  </div>
                </div>
                <div className="flex justify-between mt-6">
                  <Button variant="outline" onClick={() => setCurrentStep('payment')}>
                    Back
                  </Button>
                  <Button onClick={handlePaymentSubmit}>
                    Confirm Purchase
                  </Button>
                </div>
              </div>
            )}

            {/* Completion Step */}
            {currentStep === 'complete' && (
              <div className="text-center">
                <div className="bg-green-100 text-green-800 p-4 rounded-lg mb-6">
                  <h3 className="font-bold text-xl mb-2">Purchase Successful!</h3>
                  <p>Your diamonds/membership will be added to your account shortly.</p>
                </div>

                <div className="bg-gray-100 p-4 rounded-lg text-left mb-6">
                  <h4 className="font-bold mb-2">Transaction Details</h4>
                  <p>Transaction ID: {transactionId}</p>
                  <p>Account ID: {accountId}</p>
                  <p className="mt-2">
                    {selectedTab === 'diamonds' 
                      ? `Total Diamonds: ${totalDiamonds}`
                      : `Membership: ${membershipOptions.find(m => m.id === selectedPackages[0])?.name}`}
                  </p>
                  <p>Amount Paid: Rs. {totalPrice}</p>
                </div>

                <Button onClick={resetPurchase}>
                  Make Another Purchase
                </Button>
              </div>
            )}
          </CardContent>
        </Card>
      </div>
    </div>
  )
}
