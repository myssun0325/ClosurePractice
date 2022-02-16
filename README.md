# ClosurePractice
서위프트 연습.!
기록: 오픈소스 스터디하다 스위프트 초보인거 같아서 남겨놓느 그런 리드미

```swift
class FakePlayer {
    var playClosure: ((String) -> Void)?
}

protocol Player: AnyObject {
    var fakePlayer: FakePlayer { get }
}

extension Player {
    func setPlayBlock(_ play: ((_ value: String, _ sender: Self) -> Void)?) {
        if let play = play {
            fakePlayer.playClosure = { [weak self] value in
                guard let self = self else { return }
                play(value, self)
            }
        }
    }
}

class Mason: Player {
    let fakePlayer: FakePlayer = FakePlayer()
    
    var text: String?
    
    func play(_ closure: ((String) -> Void)?) {
        guard let closure = closure else { return }
        setPlayBlock { [weak self] value, _ in
            guard let self = self else { return }
            self.text = value
            closure(value)
        }
    }
}

```
