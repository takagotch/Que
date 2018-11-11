### Que
---
https://github.com/chanks/que

```
gem 'que'
bundle
gem install que

que -h
```

```ruby
class CreateQueSchema < ActiveRecord::Migration[5.0]
  def up
    Que.migrate!(version: 4)
  end
  def down
    Que.migrate!(version: 0)
  end
end

# app/jobs/charge_credit_card.rb
class ChargeCreditCard < Que::Job
  self.run_at = proc { 1.minute.from_now }
  self.priority = 10
  def run(credit_card_id, user_id:)
    user = User.find(user_id)
    card = CreditCard.find(credit_card_id)
    User.transaction do
      user.update charged_at: Time.now
      destroy
    end
  end
end

CreditCard.transaction do
  card = CreditCard.create(params[:credit_card])
  ChargeCreditCard.enqueue(card.id, user_id: current_user.id)
end

ChargeCreditCard.enqueue card.id, user_id: current_user.id, run_at: 1.day.from_now, priority: 5

for i in {1..1000}; do SEED=#i bundle exec rake; done
for i in {1..1000}; do LOG_SPEC=true SEED=328 bundle exec rake; done
```

```
```
