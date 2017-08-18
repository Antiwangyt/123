using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApplication2
{
    /// <summary>
    /// 卡片花色枚举
    /// </summary>
    public enum CardColor
    {
        UnKnow,
        Heart,
        Spade,
        Suqre,
        Flower
    }

    public enum CardNumber
    {
        UnKnow,
        One,
        Two,Three,Four,Five,Six,Seven,Eight,Nine,Ten,Jack,Queen,King
    }
    /// <summary>
    /// 类定义：
    /// 属性（一个对象所具有的特征类型，特征值为属性值）
    /// 方法（一个对象所具有的能力）
    /// 事件（对象本省做出的事件对外部产生影响）
    /// </summary>
    /// 归纳  类比  引喻 
    public class Card
    {
        public static Card Monkey = new Card() { monkeyKey="monkey"};
        public static Card MasterMonkey = new Card() { monkeyKey="masterMonkey"};

        private string monkeyKey;

        public CardColor Color { get; }

        public CardNumber Number { get;  }

        private Card()
        {
             
        }

        public Card(CardColor color, CardNumber number)
        {
            Color = color;
            Number = number;
                
        }

        public bool IsMasterMonkey()
        {
            return monkeyKey == "masterMonkey";
        }

        public bool IsMonkey()
        {
            return monkeyKey == "monkey";
        }
    }

    public class DeckCard
    {
        public IReadOnlyList<Card> Cards { get; private set; }

        public DeckCard()
        {
           var  cards = new List<Card>();
            for (int i = 1; i < 5; i++)
            {
                for (int j = 1; j < 14; j++)
                {
                    cards.Add(new Card((CardColor)i, (CardNumber)j));
                }
            }
            cards.Add(Card.Monkey);
            cards.Add(Card.MasterMonkey);
        }

        private static Random r = new System.Random();

        /// <summary>
        /// 洗牌动作
        /// </summary>
        public void Random()
        {
            List<Card> tempCardList = new List<Card>();
            for (int i = 0; i < Cards.Count; i++)
            {
                 
                var randomIndex = r.Next(Cards.Count);
                var tempCard = Cards[randomIndex];
                while (tempCardList.Contains(tempCard))
                {
                    randomIndex = r.Next(Cards.Count);
                    tempCard = Cards[randomIndex];
                }
                tempCardList.Add(tempCard);
            }
            Cards = tempCardList;


        }

        public event EventHandler<DealEventAgrs> DealListisen;

        /// <summary>
        /// 发牌
        /// </summary>
        public void Deal(int dealCount)
        {
            for (int i = 0; i < Cards.Count; i++)
            {
                var number = i % dealCount + 1;
                if (DealListisen!=null)
                {
                    DealListisen(this,new DealEventAgrs(Cards[i],number));
                }
            }
        }

        public void Sort() {
            for (int i = 0; i < 52; i++) {
                Card poAll =  (Card)Cards[i];
            }
        }

        public  class DealEventAgrs : EventArgs
        {
            public Card Card { get;  }

            public int Number { get; }

            public DealEventAgrs(Card card, int number)
            {
                Card = card;
                Number=number;
            }
        }
    }

}
