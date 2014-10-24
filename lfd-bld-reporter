#! /usr/bin/perl

use strict;
use warnings;
use Config::Tiny;
use LWP::UserAgent;

my $ip      = $ARGV[0] || '';
my $ports   = $ARGV[1] || '';
my $perm    = $ARGV[2] || '';
my $inout   = $ARGV[3] || '';
my $ttl     = $ARGV[4] || '';
my $msg     = $ARGV[5] || '';
my $logs    = $ARGV[6] || '';
my $trigger = $ARGV[7] || '';

my $config = Config::Tiny->new;
$config    = Config::Tiny->read('/etc/lfd-bld-reporter/config.ini');

my $apikey = $config->{submitter}->{apikey};
my $server = $config->{submitter}->{server};
my $smth   = $config->{submitter}->{something};

die("API Key is not set, stopped") unless $apikey;
die("Server is not set, stopped") unless $server;

my %services = (
	LF_SSHD          => 'sshd',
	LF_FTPD          => 'ftpd',
	LF_SMTPAUTH      => 'smtpd',
	LF_POP3D         => 'pop3d',
	LF_IMAPD         => 'imapd',
	LF_HTACCESS      => 'httpd',
	LF_MODSEC        => 'httpd',
	LF_BIND          => 'named',
	LF_SUHOSIN       => 'httpd',
	LF_APACHE_404    => 'httpd',
	CT_LIMIT         => 'portflood',
	PS_LIMIT         => 'firewall',
	LF_CUSTOMTRIGGER => '?',
);

my $service = $services{$trigger} || '';

if ($service) {
	if ('?' == $service) {
		if ($msg =~ /^\(ssh/) {
			$service = 'sshd';
		}
		elsif ($msg =~ /^\(wplogin\)/) {
			$service = 'badbot';
		}
		else {
			$service = '';
		}
	}

	if ($service) {
		my $ua = LWP::UserAgent->new();
		$ua->post('http://www.blocklist.de/en/httpreports.html', { 'apikey' => $apikey, 'format' => 'text', 'ip' => $ip, 'logs' => $logs, 'server' => $server, 'service' => $service });
	}
}
