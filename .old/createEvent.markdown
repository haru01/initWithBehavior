#v1

//
//  BrowseGithub - BGEventSpec.m
//  Copyright 2012年 yaakaito. All rights reserved.
//
//  Created by: yaakaito
//

#import "Kiwi.h"
#import "BGEvent.h"
#import "NSBundle+Specs.h"

SPEC_BEGIN(BGEventSpec)

describe(@"Event", ^{
    __block BGEvent *event;
    
    context(@"をJSONから作ったとき", ^{
//        {
//            "type": "WatchEvent",
//            "payload": {
//                "action": "started"
//            },
//            "created_at": "2012-09-10T02:25:27Z",
//            "actor": {
//                "login": "yaakaito",
//                "avatar_url": "https://secure.gravatar.com/avatar/8ba73f340ca40f9aa49a521fdc9512ce?d=https://a248.e.akamai.net/assets.github.com%2Fimages%2Fgravatars%2Fgravatar-user-420.png",
//                "url": "https://api.github.com/users/yaakaito",
//                "id": 283562,
//                "gravatar_id": "8ba73f340ca40f9aa49a521fdc9512ce"
//            },
//            "public": true,
//            "id": "1595546744",
//            "repo": {
//                "url": "https://api.github.com/repos/gkz/LiveScript-style-guide",
//                "name": "gkz/LiveScript-style-guide",
//                "id": 5741252
//            }
//        }
        beforeAll(^{
            id json = [NSBundle jsonObjectForResourceName:@"event"];
            event = [[BGEvent alloc] initWithJSON:json];
        });
        
        it(@"は、イベントの種類を文字列で持つ", ^{
            [[event.typeString should] equal:@"WatchEvent"];
        });
        
        it(@"は、ペイロードをJSON(Dictionary)形式で持つ", ^{
            [event.payload shouldNotBeNil];
            [[event.payload should] beKindOfClass:[NSDictionary class]];
            [[[event.payload objectForKey:@"action"] should] equal:@"started"];
        });
        
        it(@"は、アクター情報を持つ", ^{
            [[event.actorLogin should] equal:@"yaakaito"];
            [[[event.actorGravatarUrl absoluteString] should] equal:@"https://secure.gravatar.com/avatar/8ba73f340ca40f9aa49a521fdc9512ce?d=https://a248.e.akamai.net/assets.github.com%2Fimages%2Fgravatars%2Fgravatar-user-420.png"];
            
        });
        
        it(@"は、対象のリポジトリ情報を持つ", ^{
            [[event.repositoryName should] equal:@"gkz/LiveScript-style-guide"];
        });
        
        it(@"は、情報からメッセージを作成する", ^{
            NSString *message = @"WatchEvent : yaakaito started gkz/LiveScript-style-guide";
            [[[event message] should] equal:message];
        });
    });
});

SPEC_END



#import "BGEvent.h"

@interface BGEvent()
@property (nonatomic, strong) NSString *typeString;
@property (nonatomic, strong) id payload;
@property (nonatomic, strong) NSString *actorLogin;
@property (nonatomic, strong) NSURL *actorGravatarUrl;
@property (nonatomic, strong) NSString *repositoryName;
@end

@implementation BGEvent

- (id)initWithJSON:(id)json {
    
    self = [super init];
    if (self) {
        self.typeString = [json objectForKey:@"type"];
        self.payload = [json objectForKey:@"payload"];
        self.actorLogin = [[json objectForKey:@"actor"] objectForKey:@"login"];
        self.actorGravatarUrl = [NSURL URLWithString:[[json objectForKey:@"actor"] objectForKey:@"avatar_url"]];
        self.repositoryName = [[json objectForKey:@"repo"] objectForKey:@"name"];
    }
    return self;
}

- (NSString *)message {
    return [NSString stringWithFormat:@"%@ : %@ %@ %@", self.typeString, self.actorLogin, [self.payload objectForKey:@"action"], self.repositoryName];
}

@end


メッセージビルダー的な話へ
